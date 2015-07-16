# Introduction #

Fabrication Console is AIR based application for debugging & developer support for fabrication based applications.

# Details #

![http://code.szemraj.eu/fabrication/console/images/preview/console.jpg](http://code.szemraj.eu/fabrication/console/images/preview/console.jpg)

Console is divided into three parts:

  * **Flow** console, where all important and framework-related events are logged ( actors registrations, commands executions, notification sending etc. ). By clicking small icon on the right you can display a few details about given action.
  * **Log** console, where developer can log custo messages ( filtered by levels ) and inspect objects.
  * **Error** console, where problems reports from framework are displayed.

# Flow console #

To enable flow console logging from your application you have to override **fabricationLoggerEnabled** method in your application ( module ) main class ( fabricator ):

**Example.as**
```
<fabrication:FlexHaloApplication
	xmlns:mx="http://www.adobe.com/2006/mxml"
	xmlns:fabrication="http://puremvc.org/utilities/fabrication/2010"
	>
	<mx:Script>
		<![CDATA[

        import shell.controller.FabricationRoutingDemoShellStartupCommand;

        override public function getStartupCommand():Class  {
            return FabricationRoutingDemoShellStartupCommand;
        }
        override public function get fabricationLoggerEnabled():Boolean   {
            return true;
        }
		]]>
	</mx:Script>
</fabrication:FlexHaloApplication>
```

![http://code.szemraj.eu/fabrication/console/images/preview/flow_console_annotations.jpg](http://code.szemraj.eu/fabrication/console/images/preview/flow_console_annotations.jpg)

  1. logged actions list
  1. click these icons for more details
  1. example of detail window for mediator registration action


# Log console #

To use custom loggins you have to create **Logger** instance and add console channel to it:
```
  var logger:Logger = Log.getLogger( "myLogger" );
  logger.addChannel( FabricationLoggerChannel.getInstance() );
```
then, you can log messages using five levels ( wich then can be filtered in Log console ) or inspect object:
```
   logger.debug( "debug message" );
   logger.info( "debug message" );
   logger.warn( "debug message" );
   logger.error( "debug message" );
   logger.fault( "debug message" );

   logger.inspectObject( [ "hello", "from", "fabrication" ], "myArrayOfStrings" );
```
You can use **Log** class static methods for more convenient logging. To do so you have to remember to create global logger instance in first place ( logger instance without _name_ ):
```
  Log.getLogger().addChannel( FabricationLoggerChannel.getInstance() );
  Log.debug( "debug message" );
  Log.info( "debug message" );
  Log.warn( "debug message" );
  Log.error( "debug message" );
  Log.fault( "debug message" );
```

**TIP:** you can write you own channel and add it to any instance of **Logger** class. Remember that your channel has to implement **ILogChannel** interface ( http://code.google.com/p/fabrication/source/browse/framework/trunk/src/org/puremvc/as3/multicore/utilities/fabrication/logging/channel/ILogChannel.as ).

![http://code.szemraj.eu/fabrication/console/images/preview/log_console_annotations.jpg](http://code.szemraj.eu/fabrication/console/images/preview/log_console_annotations.jpg)

  1. custom logs window
  1. object inspector grid.

# Error console #

In here any message from fabrication about something wrong just happened is logged. At this moment these two situations are reported:
  * there is a problem resolving reaction ( typo in reactTo/trapTo methon in sourceName place, event source is null etc. )
  * notification has been sent and no observator is registered for it ( including commands and interceptors ).

**If you have any idea about more situations that could be prompted in here, let me know**

![http://code.szemraj.eu/fabrication/console/images/preview/error_console.jpg](http://code.szemraj.eu/fabrication/console/images/preview/error_console.jpg)

# Download #

You can download FabricationConsole AIR package from here:
[FabricationConsole](http://code.szemraj.eu/fabrication/console/console.air). Application supports autoUpdate functionality.