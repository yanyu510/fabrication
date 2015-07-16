# Introduction #

RPC services extension ( fabrication\_rpc.swc ) is set of classes wich allows you to use custom dependencies mechanism for providing easy and flexible way to define service dependency in remote proxies. Using this solution you can switch between various services dependency providers ( test, production etc. ) without changing anything in your model implementation.

# Requirements #

RPC services extension requires Fabrication 0.7.5 ( with custom dependencies mechanism ) and access to mx.rpc.`*` package.

# Usage scenario #

Let's say that you need to implement simple login service in your application. For testing purposes you do not connect directly to serverside service but you want to use some kind of mockup data in exchange.

## services providers ##

We will use [ServicesProvider](http://code.google.com/p/fabrication/source/browse/framework/trunk/src_rpc/org/puremvc/as3/multicore/utilities/fabrication/services/ServicesProvider.as) ( mx.rpc.AbstractService container ) to define production and test providers with one defined service - "loginService":

**ProductionServicesProvider.mxml**
```
<fabrication:ServicesProvider xmlns:fx="http://ns.adobe.com/mxml/2009"
                                  xmlns:fabrication="http://puremvc.org/utilities/fabrication/2010"
                                  xmlns:mx="library://ns.adobe.com/flex/mx">
     <mx:RemoteObject id="loginService"/>
</fabrication:ServicesProvider>
```

**TestServicesProvider.mxml**
```
<fabrication:ServicesProvider xmlns:fx="http://ns.adobe.com/mxml/2009"
                                  xmlns:fabrication="http://puremvc.org/utilities/fabrication/2010"
                                  xmlns:services="services.*">
      <services:LoginMockService id="loginService"/>
</fabrication:ServicesProvider>
```

**LoginMockService.as**
```
package services {
    import mx.rpc.AsyncToken;

    import org.puremvc.as3.multicore.utilities.fabrication.services.FabricationMockService;

    public class LoginMockService extends FabricationMockService {


        private var logins:Array = [ { login:"eric", pass:"cartman" }, { login:"stan", pass:"marsh" }];

        public function getLogins():AsyncToken
        {
            return createMockResult(logins, 2000);
        }
    }
}
```

LoginMockService class extends [FabricationMockService](http://code.google.com/p/fabrication/source/browse/framework/trunk/src_rpc/org/puremvc/as3/multicore/utilities/fabrication/services/FabricationMockService.as) class wich allows execution custom defined calls.

## adding providers strategy ##

In application class we can decide which one of providers we want to use ( using conditional compilation etc. ):

**Application.mxml**
```
<fabrication:FlexApplication xmlns:fx="http://ns.adobe.com/mxml/2009"
                             xmlns:s="library://ns.adobe.com/flex/spark"
                             xmlns:fabrication="http://puremvc.org/utilities/fabrication/2010"
                             xmlns:mx="library://ns.adobe.com/flex/mx"

        >
    <fx:Script>
        <![CDATA[

        import controller.ApplicationStartupCommand;

        override public function getStartupCommand():Class
        {

            return ApplicationStartupCommand;

        }

        override public function getClassByName(path:String):Class
        {
            return getDefinitionByName(path) as Class;
        }

        override public function get fabricationLoggerEnabled():Boolean
        {
            return true;
        }

        override public function get dependencyProviders():Array
        {
            return [ ProductionServicesProvider ]
           // or return [ TestServicesProvider ]
        }
        
        

        
		]]>
	</fx:Script>
</fabrication:FlexApplication>
```

## remote proxy ##

[FabricationRemoteProxy](http://code.google.com/p/fabrication/source/browse/framework/trunk/src_rpc/org/puremvc/as3/multicore/utilities/fabrication/patterns/proxy/FabricationRemoteProxy.as) with service injection mechanism provides us an easy way to execute remote calls.

**LoginProxy.as**
```
package model {
    import mx.rpc.events.ResultEvent;

    import notifications.LOGINS_READY;

    import org.puremvc.as3.multicore.utilities.fabrication.patterns.proxy.FabricationRemoteProxy;


    public class LoginProxy extends FabricationRemoteProxy {

        static public const NAME:String = "LoginProxy";


        [InjectService("loginService")]
        public var loginService:LoginService;

        public function LoginProxy()
        {
            super(NAME);
        }

        public function getLogins():void {

            excuteServiceCall( loginService.getLogins(), onLoginsComplete, onLoginsFault );

        }

        private function onLoginsFault( fault:Object ):void
        {
        }

        private function onLoginsComplete( result:Object ):void
        {
            sendNotification( LOGINS_READY, ( result as ResultEvent ).result );

        }



    }
}
```

As you can see **you don't have to change anything in proxy class to switch to another remote data source**. Because we're always injecting service "loginService" its implementation depends on instance created by added service provider.

# Examples #
  1. RPCExample [Demo](http://fabrication.googlecode.com/svn/examples/rpc_example/bin/index.html) [Source](http://code.google.com/p/fabrication/source/browse/#svn/examples/rpc_example)