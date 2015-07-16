


---


# Introduction #
Fabrication reduces the switch-case complexity in a Mediator's handleNotification by using reflection. Instead of specifying the notification interests array with handleNotification you write `respondTo<NotificationName>` methods inside the mediators.

# Requirements #
This feature uses the Flash Player's reflection api and when using modular applications needs an additional getClassByName method in your application's main class. This method is needed to workaround the sandboxing of getDefinitionByName.

The following code must be present along with your getStartupCommand method for reflection to work.
```
override public function getClassByName(path:String):Class {
	return getDefinitionByName(path) as Class;	
}
```

# The respondTo convention #
The respondTo convention assumes that for any method named `respondTo<NotificationName>` in your mediator, you are interested in the corresponding `notificationName` PureMVC notification. The convention requires that notification names be lower camel-cased when sending. And upper camel-cased in the respondTo handler function.

You can specify n number of respondTo handlers for the notifications that you want to handle in your mediator. Each is treated as an interest in the notification. Some typical respondTo handlers are listed below.

Handles the **`ready`** notification
```
public function respondToReady(note:INotification):void {

}
```

Handles the **`available`** notification
```
public function respondToAvailable(note:INotification):void {

}
```

# Using custom notification classes #
When using custom notification classes, you have the added benefit of strong typing the notification argument to the type of your custom notification class.

Sends a custom notification
```
sendNotification(new CaptionNotification(CaptionNotification.CAPTION_CHANGED));
```

For **`CaptionNotification.CAPTION_CHANGED = "captionChanged"`** the corresponding respondTo handler is,
```
public function respondToCaptionChanged(note:CaptionNotification):void {

}
```

# Examples #
  1. Simple Undo - HeadingMediator [Browse](http://code.google.com/p/fabrication/source/browse/examples/simple_undo/src/main/flex/view/HeadingMediator.as) [SVN](http://fabrication.googlecode.com/svn/examples/simple_undo)
  1. Simple Routing - MessageControlBarMediator [Browse](http://code.google.com/p/fabrication/source/browse/#svn/examples/resolver_tab_navigator/src/main/flex) [SVN](http://fabrication.googlecode.com/svn/examples/simple_routing)