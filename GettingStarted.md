


---


# 1. Requirements #
  * Fabrication uses the PureMVC Multicore and Pipes libraries. These need to be in your application's classpath as swcs or in source form, depending on your preference.
    1. [PureMVC Multicore](http://trac.puremvc.org/PureMVC_AS3_MultiCore).
    1. [PureMVC Pipes utility](http://trac.puremvc.org/Utility_AS3_MultiCore_Pipes).

# 2. Create your application's startup command class #
The first step is to create the startup command for your application. To get the most benefit out of Fabrication, your Mediators, Proxies and Commands should extend base classes provided by Fabrication instead of the standard onces. However you can also the PureMVC base classes as well. Lets extend the SimpleFabricationCommand class in this instance. StartupCommand classes must be named in the format `[MyApplicationName]StartupCommand`. This allows Fabrication to infer the application's name from its startup command. The code below creates an empty startup command class for the application _HelloFabrication_.

```
package controller {
	import org.puremvc.as3.multicore.interfaces.INotification;
	import org.puremvc.as3.multicore.utilities.fabrication.patterns.command.SimpleFabricationCommand;	

	public class HelloFabricationStartupCommand extends SimpleFabricationCommand {

		override public function execute(note:INotification):void {
		}
	}
}
```

# 3. Create your application's main entry point #
Depending on your environment you need to created the main class for your application. Instead of instantiating the facade manually in your main application you use the built in fabrication classes as the base class of your main application class. Depending on the environment your are running in this can be FlexApplication, FlashApplication, FlexModule, or AirApplication. In your application class you need to provide a reference to the application's startup command in the getStartupCommand method.

### For Flex ###
For the Flex environment the main application code would look like
```
<?xml version="1.0" encoding="utf-8"?>
<fab:FlexApplication
	xmlns:fab="org.puremvc.as3.multicore.utilities.fabrication.components.*" 
	xmlns:mx="http://www.adobe.com/2006/mxml">
	
	<mx:Script>
		<![CDATA[
			import controller.HelloFabricationStartupCommand;
			
			override public function getStartupCommand():Class {
				return HelloFabricationStartupCommand;
			}
                        
                        override public function getClassByName(path:String):Class  {
                                return getDefinitionByName(path) as Class;
                        }

			
		]]>
	</mx:Script>
	
</fab:FlexApplication>
```

### For Flex Modules ###
In Flex module based applications the main application code would look like,
```
<?xml version="1.0" encoding="utf-8"?>
<fab:FlexModule
	xmlns:fab="org.puremvc.as3.multicore.utilities.fabrication.components.*" 
	xmlns:mx="http://www.adobe.com/2006/mxml">
	
	<mx:Script>
		<![CDATA[
			import controller.HelloFabricationStartupCommand;
			
			override public function getStartupCommand():Class {
				return HelloFabricationStartupCommand;
			}

                        override public function getClassByName(path:String):Class  {
                                return getDefinitionByName(path) as Class;
                        }
			
		]]>
	</mx:Script>
	
</fab:FlexModule>
```

### For Flash / Pure AS3 ###
In Flash or pure AS3 applications the main application code would look like,
```
package {
	import controller.HelloFabricationStartupCommand;

	import org.puremvc.as3.multicore.utilities.fabrication.components.FlashApplication;		

	public class HelloFabrication extends FlashApplication {

		override public function getStartupCommand():Class {
			return HelloFabricationStartupCommand;
		}

                override public function getClassByName(path:String):Class  {
                         return getDefinitionByName(path) as Class;
                }
	}
}
```

### For AIR ###
In AIR based flex applications the main application code would look like,
```
<?xml version="1.0" encoding="utf-8"?>
<fab:AirApplication
	xmlns:fab="org.puremvc.as3.multicore.utilities.fabrication.components.*" 
	xmlns:mx="http://www.adobe.com/2006/mxml">
	
	<mx:Script>
		<![CDATA[
			import controller.HelloFabricationStartupCommand;
			
			override public function getStartupCommand():Class {
				return HelloFabricationStartupCommand;
			}

                       override public function getClassByName(path:String):Class  {
                                return getDefinitionByName(path) as Class;
                        }
			
		]]>
	</mx:Script>
	
</fab:AirApplication>
```

# 4. Create your application mediator #
Next create the main mediator of your application. When using Flex you should extend the FlexMediator class and for flash use the FlashMediator. And add register the mediator in the startup command class.

In view/HelloFabricationMediator.as
```
package view {
	import org.puremvc.as3.multicore.utilities.fabrication.patterns.mediator.FlexMediator;		

	public class HelloFabricationMediator extends FlexMediator {

		static public const NAME:String = "HelloFabricationMediator";

		public function HelloFabricationMediator(viewComponent:Object) {
			super(NAME, viewComponent);
		}
	}
}
```

In controller/HelloFabricationStartupCommand.as
```
override public function execute(note:INotification):void {
	registerMediator(new HelloFabricationMediator(note.getBody()));
}
```

# 5. Send a `now` notification #
Next we send a `now` notification using the PureMVC `sendNotification` method. We can do this from either the mediator or command. For this example we will use the startup command itself.

In controller/HelloFabricationStartupCommand.as
```
override public function execute(note:INotification):void {
	registerMediator(new HelloFabricationMediator(note.getBody()));
	
	sendNotification("now", new Date());
}
```

# 6. Respond to the `now` notification #
In order to respond to the `now` notification we will use the respondTo syntax instead. By adding a respondToNow handler in the main mediator we indicate our interest in the `now` notification. The notification name must use lower camel case and the time of sending the notification, and is converted to upper camel case when handling the notification in the respondTo method.

In view/HelloFabricationMediator
```
public function respondToNow(note:INotification):void {
	trace("respondToNow time=" + note.getBody());
}
```