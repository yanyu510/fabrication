


---


# Introduction #
Interceptors provide means to alter the normal flow of PureMVC Notifications before they reach Commands and Mediators. With Interceptors you can,

  * Drop any notification based on any custom notification.
  * Modify the notification payload.
  * Trap the original notification and send a different notification in its place.
  * Sends N notifications while allowing the source notification to proceed.
  * Interceptors are similar to commands and have access to the rest of PureMVC actors. So any part of the application can also be modified.

# Requirements #
This feature needs Fabrication version 0.6 or above.

# Registering Interceptors #
Interceptors are registered using the registerInterceptor method. This method is available on the FabricationFacade or within subclasses of SimpleFabricationCommand.

The syntax is,
```
registerInterceptor(noteName:String, clazz:Class, parameters:Object = null)
```

  * noteName : The name of the notification to register.
  * clazz : The class reference to the interceptor.
  * parameters : Optional parameters to be assigned to the interceptor when it is instantiated. This parameter is useful to reuse the same interceptor class with different notification names.

```
// registers the interceptor for the save notification name.
registerInterceptor("save", MyInterceptor);

// registers the interceptor with save notification with extra parameters
registerInterceptor("save", MyInterceptor, {foo:"bar"});
```

# Implementing Interceptors #
Interceptors need to extend the AbstractInterceptor class and implement a concrete **intercept** method. The **intercept** method will be
called when the notification that it was registered for is fired anywhere in the application. The interceptor instance has access to the following properties at the time the intercept method is called,

  * notification : The notification object to intercept.
  * parameters : Optional parameters that were specified when the interceptor was registered.
  * processor : The processor object provides means to proceed, abort or skip the notification. The proceed, abort and skip methods in the AbstractInterceptor are alias methods to processor.<proceed|abort|skip>.

Once the intercept method is called the notification will not reach the rest of the PureMVC actors until you call one of the following methods.

  * proceed(note:INotification):void : This method should be called if you wish to allow the notification to continue. If the note argument is not specified the original notification object is used. If another notification object is specified as the note argument that notification will be sent in place of the original notification.
  * abort():void : The notification will be dropped.
  * skip():void : This method indicates that processing within this interceptor has finished. Use this method when multiple interceptors are registered with the same notification name.

The interceptor processing can be asynchronous. In the following example an alert is show when the notification is intercepted. If Ok was clicked in the alert the notification is allowed to proceed otherwise it is aborted.

```
override public function intercept():void {
	Alert.yesLabel = "Ok";
	Alert.cancelLabel = "Cancel";
	Alert.show("Are you sure?", "Confirm", Alert.OK | Alert.CANCEL, null, closeListener);
}

private function closeListener(event:CloseEvent):void {
	if (event.detail == Alert.OK) {
		proceed();
	} else {
		abort();
	}
}
```

# Examples #
  1. Simple Interceptor [Browse](http://code.google.com/p/fabrication/source/browse/examples/interceptor_demo/src) [SVN](http://fabrication.googlecode.com/svn/examples/simple_undo)
  1. StateMachine utility with Fabrication [Browse](http://code.google.com/p/fabrication/source/browse/examples/simple_fsm/src/interceptor/StateMachineInterceptor.as) [SVN](http://fabrication.googlecode.com/svn/examples/simple_fsm)