# Introduction #

With version 0.7.1 a brand new mechanism is being introduced - IoC for Fabrication actors. It simplyfies the way you can retrieve mediators/proxies instances within body of your classes.


# Requirements #

This feature needs Fabrication version 0.7.1 or above.

# Usage Scenarios #

Let say you have your startupCommand like this one:

**MyApplicationStartupCommand.as**
```
package  controller {
    import org.puremvc.as3.multicore.interfaces.INotification;
    import org.puremvc.as3.multicore.utilities.fabrication.patterns.command.SimpleFabricationCommand;

    import spark.components.Application;

    public class MyApplicationStartupCommand extends SimpleFabricationCommand {


        override public function execute(notification:INotification):void
        {
            registerProxy( MyApplicationProxy1( "MyAppProxy1", { someData:"DataFromProxy1" } );
            registerProxy( MyApplicationProxy2( "MyAppProxy2", { someData:"DataFromProxy2" } );
        }
    }
}
```

and your application mediator, when you want to get reference to one of registered proxies:

**MyApplicationMediator.as**
```
package view {
	import org.puremvc.as3.multicore.utilities.fabrication.patterns.mediator.FlexMediator;		

	public class MyApplicationMediator extends FlexMediator {
		
		static public const NAME:String = "MyApplicationMediator";
		
		public function MyApplicationMediator(viewComponent:Object) {
			super(NAME, viewComponent);
		}
					
		override public function onRegister():void {
                        super.onRegister();
			var myProxy:MyApplicationProxy1 = retrieveProxy( "MyAppProxy1" ) as MyApplicationProxy1;
		}
		
	}
}
```
## Injection by class ##
Using new IoC mechanism you can do something like this:

**MyApplicationMediator.as**
```
package view {
	import org.puremvc.as3.multicore.utilities.fabrication.patterns.mediator.FlexMediator;		

       	public class MyApplicationMediator extends FlexMediator {
	
		static public const NAME:String = "MyApplicationMediator";

                [InjectProxy]
                public var proxy:MyApplicationProxy;
		
		public function MyApplicationMediator(viewComponent:Object) {
			super(NAME, viewComponent);
		}
				
	}
}
```

## Injection by type ##
Injecting actors by type ( and it's name ) give you possibility to simple and quick way to switch between instances of same type. For example, we can simply inject instance of some proxy ( IProxy ) named "MyAppProxy1":

**MyApplicationMediator.as**
```
package view {
	import org.puremvc.as3.multicore.utilities.fabrication.patterns.mediator.FlexMediator;		

       	public class MyApplicationMediator extends FlexMediator {
		
		static public const NAME:String = "MyApplicationMediator";

                [InjectProxy(name="MyAppProxy1"]
                public var proxy:IProxy;
		
		public function MyApplicationMediator(viewComponent:Object) {
			super(NAME, viewComponent);
		}
				
	}
}
```

We can change metadata name argument to "MyAppProxy2" to have another instance injected.

# Additional info #

  * you can use the same mechanism for mediators injection ( `[` InjectMediator `]` metatag ).
  * By design, you can inject mediators and proxies into commands and mediators. Proxy can have injected only other proxies.
  * In commands all injected references are disposed when command execution is finished. In mediators all references are disposed on mediator remove. To clean same references in your proxy you have to use Proxy.dispose method.

# Examples #
  1. IoC mechanism example [Demo](http://fabrication.googlecode.com/svn/examples/ioc_mechanism_example/bin/index.html) [Source](http://code.google.com/p/fabrication/source/browse/#svn/examples/ioc_mechanism_example)