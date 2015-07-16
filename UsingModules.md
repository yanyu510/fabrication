


---


# Introduction #
Fabrication provides a very simple way of communicating between modules. It uses the pipes utility as the base layer for communication. This inter-module piping is done automatically and module communication is simplified into standard PureMVC notifications.

# Requirements #
This feature uses the Flash Player's reflection api and when using modular applications needs an additional getClassByName method in your application's main class. This method is needed to workaround the sandboxing of getDefinitionByName.

The following code must be present along with your getStartupCommand method for reflection to work.
```
override public function getClassByName(path:String):Class {
	return getDefinitionByName(path) as Class;	
}
```

# Loading Modules #
The approach to loading modules differs based on its environment. For loading Flex based modules, Flex specific classes need to be used. Whereas for Flash based modules plain Loader objects are sufficient.

## Using Flex Module Loader for Flex Modules ##
Fabrication provides a FlexModuleLoader for loading flex modules. This class should be used when the child module extends the FlexModule fabrication class. FlexModuleLoader extends the native ModuleLoader class so the loading api and events are the same.

Loads the module, my\_module.swf using the FlexModuleLoader
```
var moduleLoader:FlexModuleLoader = new FlexModuleLoader();

moduleLoader.url = "my_module.swf";
moduleLoader.loadModule();
```

## Using Module Manager for Flex Modules ##
Flex modules can also be loaded using the ModuleManager. The ModuleManager only loads the module swf. It does not instantiate the loaded swf. That needs to be done manually after the loading has completed by invoking create on the module's factory.

Loads the module, my\_module.swf using ModuleManager.
```
var module:IModuleInfo = ModuleManager.getModule("my_module.swf");
module.addEventListener(ModuleEvent.READY, moduleReadyListener);
module.load();

private function moduleReadyListener(event:ModuleEvent):void {
	var moduleInstance:Object = event.module.factory.create();
}
```

## Using Fabrication module loader and Fabrication AIR module loader ##
You can use FabricationModuleLoader and FabricationAirModuleLoader to load module in browser or in desktop application. Both classes works similar to ModuleManager instances, but hides less important functionality from programmer. Only thing you need to do is register for ModuleEvent.READY event and retrieve module instance ( as flexModule getter ) in event handler. See example at the end of page.

## Using plain Loader for Flash ##
### Important Note ###
_The distinction between Applications and Modules is specific to Flex. Fabrication does not make any distinction between applications and modules for Flash based applications. Any application swf that extends FlashApplication can be loaded into another FlashApplication application._

Loading a flash application module can be done using the native Loader class.

Loads the flash appliction module, my\_module.swf using a Loader object.
```
loader = new Loader();
loader.load(new URLRequest("my_module.swf"));
```

# Module Configuration #
Modules must be configured before communication between modules can be performed. This is done by setting the router property on the module loader object. For Flex based modules this should be before loading by setting the property on the FlexModuleLoader instance. For flash based module, the property should be set on the Loader content after it has finished loading.

The router for an application can be obtained by the `applicationRouter` property. This property is available throughout the Fabrication base classes like FabricationMediator, SimpleFabricationCommand, etc.

By default a module is configured to communicate to all modules within a shell application. This behaviour can be overridden by setting the defaultRouteAddress of a module to the current application's module address. Doing so configures the module to send messages to the shell application that loaded it.

For Flex modules,
```
moduleLoader.router = applicationRouter;
moduleLoader.defaultRouteAddress = applicationAddress;
```

For Flash application based modules this needs to be done in the complete handler of the loader object used to load the module swf.
```
private function completeListener(event:Event):void {
	var content:FlashApplication = loader.content as FlashApplication;
	content.router = applicationRouter;
	content.defaultRouteAddress = applicationAddress;
}
```

# Sending notification between modules #
Communication between modules is done with the routeNotification method. This method is available in all the fabrication base classes like, FlexMediator, SimpleFabricationCommand, FabricationFacade, etc. The routeNotification is similar to the sendNotification method of PureMVC. However unlike sendNotification, routeNotification transports the notification across modules.

The syntax of routeNotification is,
```
routeNotification(noteName:Object, noteBody:Object, noteType:String, to:Object):void
```

To send a notification to the default module address use,
```
routeNotification("myNote");
```

Like sendNotification you can specify optional noteBody, and noteType arguments.
```
routeNotification("myNote", new Object(), "myType");
```

To send notification to a module you use the **`to`** argument. The **`to`** argument supports the following formats.

  1. `*` - Sends the notification to all modules
  1. ModuleName/`*` - Sends the notification to all instances of the module, **ModuleName**
  1. ModuleName/ModuleInstance - Sends the notification to the **ModuleInstance** instance of the module, **ModuleName**.

```
routeNotification("myNote", null, null, "*"); // to everyone
routeNotification("myNote", null, null, "A/*); // to instances of the module A
routeNotification("myNote", null, null, "A/A0"); // to instance A0 of the module A
```

# Handling notifications from modules #
The notifications sends using routeNotification are converted to plain PureMVC notifications in the destination module. They can be mapped to commands or used with respondTo handlers, or plain handleNotification methods.

Sends a **messageFromModule** notification to all modules
```
routeNotification("messageFromModule", null, null, "*");
```

Handles the **messageFromModule** notification that originated in another module.
```
public function respondToMessageFromModule(note:INotification):void {

}
```

# Sending custom notification objects between modules #
You can also send custom typed notification object between modules. This is done by specifying the custom notification object as the first and only parameter of routeNotification. Handling the notification in destination module is same as with a dynamic notification.

Sends a custom **ModuleNotification** to all modules
```
routeNotification(new ModuleNotification(ModuleNotification.MESSAGE_FROM_MODULE));
```

Handles the custom **ModuleNotification** as a custom PureMVC notification.
```
public function respondToMessageFromModule(note:ModuleNotification):void {

}
```

# Examples #
  1. Simple Routing [[Browse](http://code.google.com/p/fabrication/source/browse/#svn/examples/simple_routing/src/main/flex)] [[SVN](http://fabrication.googlecode.com/svn/examples/simple_routing)]
  1. Using Flash based modules [[Browse](http://code.google.com/p/fabrication/source/browse/#svn/examples/hello_flash_with_module/src)] [[SVN](http://fabrication.googlecode.com/svn/examples/hello_flash_with_module)]
  1. Using FabricationModuleLoader and FabricationAirModuleLoader [Demo](http://fabrication.googlecode.com/svn/examples/fabrication_module_loader_example/bin/index.html) [Source](http://code.google.com/p/fabrication/source/browse/#svn/examples/fabrication_module_loader_example)