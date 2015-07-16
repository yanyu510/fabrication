


---


# Introduction #
Starting from verion 0.7.1 you can use advanced reaction registration mechanism. From now you can use:
**reactTo`<ComponentName`>$`<EventName`>** syntax ( be aware of dollar sign between component and event name ).

# Requirements #
This feature needs Fabrication version 0.7.1 or above.

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

       /* not needed anymore
	public function get submitButton():Button {
		return viewComponent.submitButton as Button;
	}
       */

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
public function reactToSubmitButton$Click(event:MouseEvent):void {
	// submit form
}
```

# Implementation Details #

  1. To use new Registration mechanism **you don't need** to declare public getter to viewComponent child if this child is accessible outside this component. You have to remember about CamelCase naming convention.
  1. You can use capture phase by declaring method as: **trap`<ComponentName`>$`<EventName`>**
  1. All others details are the same as in standard reactions ( removing, constant style event names etc. )

# Examples #
  1. New reactions example [Demo](http://fabrication.googlecode.com/svn/examples/reactions_example/bin/index.html) [Source](http://code.google.com/p/fabrication/source/browse/#svn/examples/reactions_example)