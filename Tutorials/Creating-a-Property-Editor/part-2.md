#Tutorial - adding configuration to a property editor

##Overview
This is step 2 in our guide to building a property editor. This step continues work on the markdown editor we built in [step 1](creating-tutorial1-v7.md), but goes further to show you how you can add configuration options to the editor.


##Configuration?
So an important part of building good property editors, is to build something relatively flexible, so you can reuse it many many times, for different things; Like the rich text editor in Umbraco, that allows you to choose which buttons and stylesheets you want to use on each instance of the editor.

So an editor can be used several times, with different configurations, and that is what we will be working on now.


##package.manifest
To add configuration options to our markdown editor, open the `package.manifest` file. Right below the editor definition, paste in the following:

	prevalues: {
		fields: [
			{
				label: "Preview",
				description: "Display a live preview",
				key: "preview",
				view: "boolean"
			},
			{
				label: "Default value",
				description: "If value is blank, the editor will show this",
				key: "defaultValue",
				view: "textarea"
			}
		]
	}

**Remember to:** Separate the editor element and prevalue editor definition with a comma, or you will get a JSON error.

So what did we just add? We added a prevalue editor, with a `fields` collection. This collection contains information about the UI we will render on the datatype configuration for this editor.

So the first one gets the label "Preview" and uses the view "boolean", so this will allow us to turn preview on/off and will provide the user with a simple checkbox. The name "boolean" comes from the convention that all preview editors are stored in `/umbraco/views/prevalueeditors/` and then found via `<name>.html`

Same with the next one, only that it will provide the user with a textarea to input a default value for the editor.

Save the manifest, **restart the app pool** and have a look at the markdown datatype in Umbraco now. You should now see that you have 2 configuration options.

##Using the configuration
The next step is to gain access to our new configuration options. For this, open the `markdowneditor.controller.js` file.

Let's first add the default value functionality. Basically, when the ´$scope.model.value` is empty or *undefined*, we want to use the default value, to do that, we add the following to the very beginning of the controller:

	if($scope.model.value === null || $scope.model.value === ""){
	    $scope.model.value = $scope.model.config.defaultValue;
	}

You see whats new? - the `$scope.model.config` object is. And the other thing you will notice is that because of our configuration, we now have access to `$scope.model.config.defaultValue` which contains the configiration value for that key, it's that easy to setup and use configuration values from code.

However, you can also use these values without any JavaScript, so open the `markdowneditor.html` file instead.

Because we can also use the configuration directly in our HTML like here, where we use it to toggle the preview `<div>`, using the `ng-hide` attribute:

	<div ng-show="model.config.preview" class="wmd-panel wmd-preview"></div>


[Next - Integrating services with a property editor](part-3.md)
