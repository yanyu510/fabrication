


---


# What is Fabrication? #
Fabrication is a rapid application development extension for PureMVC. It lets you build out your PureMVC application faster without having to explicitly declare everything that you want your application to do. The goal of Fabrication is to write less code when building applications adhering to the separation of concerns of PureMVC. Fabrication is built around the Multicore version of PureMVC. This allows Fabrication to support multi-modular applications without singleton issues. The PureMVC Pipes utility is used to communicate between modules.

Fabrication is all PureMVC(Multicore). Any utilities/code that works with PureMVC will also work with Fabrication. Everything that PureMVC provides is present in Fabrication. Fabrication does not modify PureMVC, it only extends PureMVC.

# Facade extensions #
Fabrication provides a standard PureMVC Facade to kick start your application. This Facade does the work of registering and executing your startup command with a `STARTUP` notification. Instead of instantiating this facade manually in your main mxml or actionscript class, default fabrications are provided. You can extend any of the default fabrications, FlexApplication, FlashApplication, FlexModule, or AirApplication. These environment specific Fabrications manage the creation of the facade, with unique multiton keys. You only need to return a reference to your application's startup command class in a getStartupCommand method.

# Mediator extensions #
Fabrication allows registration of mediators without waiting for its corresponding viewComponent to be created. This is especially useful when using deferred component instantiation in Flex. You no longer need to code creationComplete handlers, bubble custom creation events, check for mediator registration and so on. Fabrication has a built in component resolver that detects when a component becomes available and creates the mediator for it. You only need to declare your intention of associating a mediator with a viewComponent. This feature is not limited to just deferred instantiation. Coupled with application states you can declare high level mediators registration that will get created and destroyed as the application's state changes.

# Notification extensions #
Fabrication provides mediators to detect notification interests based on intent. With Fabrication you do not need to specify the array of notifications that your mediator is interested in with `listNotificationInterests`. Instead you simply write a handler function like, `respondTo<NotificationName>`. Fabrication automatically interprets a handler function like `respondToChanged`, as a mediator's interest in the **changed** notification name. When the changed notification is send in your application this handler function will be invoked. This feature reduces the complexity in switch cases that can result in a Mediator's handleNotification. It also allows strong typing your notifications since multiple handler functions can have different argument signatures.

# Multi-module messaging extensions #
Fabrication simplifies communication between multiple modules into PureMVC Notifications. To send messages between modules you simply use the `routeNotification` method. Such routed notifications are received by the destination module as standard PureMVC notifications. You can either register them with Commands or use them in mediators with the respondTo syntax.

# Undo/Redo support #
Fabrication recognizes Undo-Redo as an integral part of rich internet applications and has extensive support for it. It provides support for n-level undos with a clean set of APIs for different types of undo operations like property changes, object adding and removal, grouping, and implicit undos.

# PureMVC enhancements #
Fabrication shortens some of the typical PureMVC programming syntax. Calls to `facade.<registerMediator|registerProxy>` etc can be performed directly with just `registerMediator, registerProxy` etc. Fabrication allows mapping multiple commands with the same notification names resulting in an implicit macro command.

# Interceptors #
Interceptors allow you to change the normal flow of PureMVC notifications before they reach Mediators and Commands. With interceptors you can modify notification payload en route, drop them based on some criteria, or send different notifications in place of the original notification.

# Reactions #
Reactions provide a way to declare component event handlers inside Mediators and subscribe to those events in the same handler. By using Reactions you also take away the need to manually remove event listeners. The event listeners are removed automatically once the source Mediator is removed.

# IoC mechanism #
New Fabrication IoC mechanism provides easy way to retrieve actors instances in classes. Using it gives easy way to switch different instances of same type, makes programmer to type less and provide automatic disposal of injections.