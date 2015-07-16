


---


# Introduction #
Reactions are the equivalent of Reflexive notification interests for component events. Instead of subscribing to component events manually using addEventListener, you declare your interest in the component event using **reactTo`<ComponentNameEventName`>** syntax.

# Requirements #
This feature needs Fabrication version 0.6 or above.

# Usage Scenario #
Consider the typical anatomy of a PureMVC Mediator class for an mxml view component show below.

**MyForm.mxml**
```
<mx:Form>
	<mx:TextInput id="emailInput" />
	<mx:Button id="submitButton" label="Submit Button"/>
</mx:Form>
```

**FormMediator.as**
```
public class FormMediator extends FlexMediator {

	static public const NAME:String = "FormMediator";

	public function FormMediator(viewComponent:Object) {
		super(NAME, viewComponent);
	}

	public function get submitButton():Button {
		return viewComponent.submitButton as Button;
	}

	public function onRegister():void {
		submitButton.addEventListener(MouseEvent.CLICK, submitButtonClicked);
	}

	private function submitButtonClicked(event:MouseEvent):void {
		// submit form
	}

}

```

Instead of subscribing to the click event you just add a click handler with the reactTo syntax.

```
public function reactToSubmitButtonClick(event:MouseEvent):void {
	// submit form
}
```

# Implementation Details #

  1. To use Reactions you must declare you target component with a getter in the Mediator using CamelCasing. In the above example the submitButton getter referenced the submitButton child of the viewComponent.
  1. In the reactTo method the submitButton must be upper CamelCased. In the above example submitButton became SubmitButton.
  1. The Component's event name must also be upper CamelCased. In the above example the click event became Click.
  1. If you have any other code implemented in the onRegister method you need to call super.onRegister.
  1. Similarly if you have any other code implemented in the onRemove method you need to call super.onRemove.

# Capture Phase #
The reactTo methods implement addEventListener with a false useCapture argument. To use the reaction with capture phase, The method prefix should be **trap**. To react to the click event in the above example in capture phase the handler would be,

```
public function trapSubmitButtonClick(event:MouseEvent):void {

}
```

# Removing Reactions #
One useful aspect of Reactions is that you do not need to use removeEventListener explicitly. Once the mediator is removed its Reactions are removed as well. If you need to remove the reaction manually, use the removeReaction method. The syntax is,

```
removeReaction(handler:Object):void
```
  * handler : Can be the string name of the handler to remove or a reference to it.

To remove the handler in the above example use,

```
// with handler reference
removeReaction(reactToSubmitButtonClick);

// with handler string
removeReaction("reactToSubmitButtonClick");
```

# Examples #
  1. InterceptorDemoMediator [Browse](http://code.google.com/p/fabrication/source/browse/examples/interceptor_demo/src/main/flex/view/InterceptorDemoMediator.as) [SVN](http://fabrication.googlecode.com/svn/examples/interceptor_demo)
  1. SimpleFsmMediator [Browse](http://code.google.com/p/fabrication/source/browse/examples/simple_fsm/src/view/SimpleFsmMediator.as) [SVN](http://fabrication.googlecode.com/svn/examples/simple_fsm)