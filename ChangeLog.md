
---

# 0.8.7 #
  * compiled framework with Apache Flex SDK 4.10.0, Adobe Air 3.8
  * removed all mx dependencies for spark-only build (Removed mx.modules.Module, mx.modules.ModuleLoader for air, flex)

# 0.8.6 #
  * compiled with last official version of Adobe SDK 4.6.0.23201B

# 0.8.4 #
  * fixed [bug #36](https://code.google.com/p/fabrication/issues/detail?id=36)
  * fixed issues with FabricationModuleLoader and FabricationAIRModuleLoader
  * introduced FabricationRemoteProxy.executeServiceCalls method

# 0.8.3 #
  * cosmetic changes:
    * ServicesProvider => FabricationServicesProvider
    * FabricationMockService => FabricationService
  * FabricationDependencyProvider and FabricationServicesProvider have initializeDependencyProvider()    protected method which is called in constructor. You can use it for ex. to registering class aliases.

# 0.8.2 #
  * fixed bug with error parsing class instances to generic object ( FabricationLogger )
  * each package contains swc file for production and rsl directory ( swc + swf ) for release purposes.

# 0.8.1 #
  * fixed bug [issue #34](https://code.google.com/p/fabrication/issues/detail?id=#34)
  * added swf file for each package to use it as RSL.

# 0.8 #
  * fixed bug with double mediator/view component instantation. Whole project recompiled using as3commons reflection library v. 1.3.3.1

# 0.7.6.4 #
  * fixed [issue #30](https://code.google.com/p/fabrication/issues/detail?id=#30) ServicesProvider default property is now array of IEventDispatcher instances instead of AbstractService
  * fixed [issue #32](https://code.google.com/p/fabrication/issues/detail?id=#32) with missing injection mechanism in AbstractInterceptor

# 0.7.6.3 #
  * fix for FabricationRemoteProxy ( data parameter was not passed through to super constructor )
  * FabricationRemoteProxy.executeServiceCall now returns AsyncToken instance.

# 0.7.6.2 #
  * introduced message filter ( lookup ) for FabricationConsole action window
  * introduced timestamp column in FabricationConsole action window
  * FabricationConsole 1.0

# 0.7.6.1 #
  * ServiceProvider is now dynamic class
  * added FabricationMediator.disposeReactions protected method
  * fixed bug, when re-registering reactions does not remove old ones

# 0.7.6 #
  * fixed typos in method names ( executeServiceCall ) and javadocs
  * added FabricationMediator.disposeView method
  * added ServiceCallStack, [FabricationRPCServiceCallStack](http://code.google.com/p/fabrication/wiki/FabricationRPCServiceCallStack)

# 0.7.5.5 #
  * fixed bug with parsing objects and circular references ( http://code.google.com/p/fabrication/issues/detail?id=18 )
# 0.7.5.4 #
  * added FabricationMediator.hasReaction method
  * fixed issues with spark components and reflexive mediators registration
  * added console warning when adding proxy with the same name as already existed one ( overwriting proxy )

# 0.7.5.3 #
  * fixed deferred instantation ( FabricationMediator registrations ) for spark components ( NavigatorContent etc. ) ( see [example](http://fabrication.googlecode.com/svn/examples/resolve_tab_navigator_spark/bin/index.html), [svn](http://code.google.com/p/fabrication/source/browse/#svn/examples/resolve_tab_navigator_spark) )
  * added auto reactions registrations on viewcomponent state change ( see [example](http://fabrication.googlecode.com/svn/examples/state_change_reactions/bin/index.html), [svn](http://code.google.com/p/fabrication/source/browse/#svn/examples/state_change_reactions) )

# 0.7.5.2 #
  * added FabricationMediator.hasReaction method
  * if developer overwrite FabricationMediator.listNotificationInterests method an warning in console will be raised about failing auto resolving notifications.
  * if FabricationMediator.onRegister method will be overwritten without call to super.onRegister() method, warning will be raised in console.
  * registrations of internal framework actors ( like FabricationDependencyPRoxy ) are no longer shown in console actions panel.
# 0.7.5.1 #
  * FabricationFacade.addDependencyProviders method has been removed. Please, use IFabrication.dependencyProviders getter to return array of providers classes or instances.
  * fixed bug with logging data >40k through LocalConnection ( fabrication logging )
  * if not specified directly FlexModule.fabricationLoggerEnabled will return inherited value from parent IFabrication object
  * style update was also made for FabricationConsole ( v. 0.1.1 ).

# 0.7.5 #
  * fixed issues with FabricationConsole ( errors on inspecting nested displayList components )
  * added [custom dependencies mechanism](http://code.google.com/p/fabrication/wiki/CustomDependencies)
  * introduced [Fabrication RPC extension](http://code.google.com/p/fabrication/wiki/FabricationRPCDependencies) based on custom dependencies mechanism

# 0.7.4.1 #
  * fixed notification action log in console
  * removed warning when facade sending note with no observers ( because observer can be outside current fabrication, like in loaded module )
# 0.7.4 #
  * added logging functionality
  * added support for FabricationConsole (v. 0.1)
  * compiled with Flex SDK 4.1.0.16076
# 0.7.3.1 #
  * fixed issue with reflection mechanism for modular applications ( http://forums.puremvc.org/index.php?topic=1763.0 )
# 0.7.3 #
  * added separate builds for flex3 (moxie) support
  * when injecting proxy if it is not registered framework will try to create & register. This should work fine if   proxy constructor has no params or has params wit default values

# 0.7.2 #
  * fixed injection mechanism in FlashMediator
  * fixed issues in new reactions mechanism
  * introduced reflexive notifications interests via namespaces

# 0.7.1 #
  * added fabrication namespace ( http://puremvc.org/utilities/fabrication/2010 )
  * AirApplication/FlexApplication for spark and AirHaloApplication/FlexHaloApplication for halo architecture
  * Introduced FabricationModuleLoader and FabricationAirModuleLoader - simplified way to manage modules using ModuleManager
  * Introduced new Reactions registration approach
  * Introduced simply IoC for fabrication actors


# 0.7 #
  * Added support for Flex4 SDK
  * Rearranged repository layout
  * Introduced FlexUnit4 tests
  * Added AsyncFabricationCommand
  * Fixed ComponentResolver and reflexive mediator registration problem in spark components
  * Fixed reflexive registration problems in components with deferred child instantiation ( TabNavigator etc. )
  * New libraries:
    * included pureMVC core, pipes and async command libs into each distribution
    * separate distributions for air, flex and pure as3
    * recompiled using Flex4 SDK and AIR2 SDK official releases

# 0.6 #
  * Added support for Notification Interceptors.
  * Added support for automatic component Reactions.
  * Added support for multiple undo history stacks within the same application.
  * Added test-class ant macro for testing a single testcase.
  * Added launch-air-app and launch-air-swf ant macro for testing an application and swf in AIR.

# 0.5.3 #
  * Fixed issue with resolve for components with id and name.
  * Added config object to IFabrication and concrete implementations.
  * Removed default startup commands, instead an error is thrown if getStartupCommand is not implemented.

# 0.5.2 #
  * Fixed double disposal in FabricationView that was throwing an exception while unloading.

# 0.5.1 #
  * Moved ComponentRouteMapper into FlexMediator from the FabricationMediator.

# 0.5 #
  * Fixed memory leaks within the framework.
  * Fixed incorrect component route caching.
  * Refactored ComponentRouteMapper object to be cached at the facade level instead of globally.
  * Fixed incorrect notification caching in the FabricationMediator.
  * Refactored message routing internals to use an explicit TransportNotification instead of the earlier dynamic implementation.
  * Added FabricationModel to improve garbage collection of proxies.
  * Added FabricationView to improve garbage collections of mediators(Thanks Jason).
  * Added HashMap object to implement consistent cache management throughout the framework.
  * Implemented IDisposable in nearly all classes in the framework.
  * routeNotification now supports typed notifications with the syntax, routeNotification(new CustomNotification(�foo�))
  * Fixed invalid auto description regular expression in SimpleUndoableCommand.
  * FabricationFacade now provides a standard caching api to store objects within the scope of a facade instance.
  * Added platform specific swcs for flex and air.
  * Moved the build system into the framework and updated example builds.

# 0.4.2 #
  * routeNotification now supports dynamic **to** parameter. Valid values are string routes and IModuleAddress objects.

# 0.4.1 #
  * Fixed FlexModule to support direct loading with ModuleManager.