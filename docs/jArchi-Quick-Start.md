# jArchi Quick Start
This is a very brief introduction to jArchi to help you get going.

**As this is in the beta stage, please do not run jArchi scripts on models without a backup! Please experiment on test models.**


## Install the jArchi plug-in

**Note - the jArchi beta binary distribution is only available for Archi donators and contributors. Please read this [blog post](https://www.archimatetool.com/blog/2018/07/02/jarchi/).**

#### Requirements
jArchi requires Archi version 4.4 or later.

#### Installation
1. Download the jArchi plug-in zip file.
2. In Archi, select "Manage Plug-ins..." from the main Help menu. From the Plug-ins Manager window, select "Install New..." and select the zip file and then, when prompted, allow Archi to restart.
3. If this is the first time you have installed the jArchi plug-in, the two tabs ("Scripts Manager" and "Scripts Console") will not be visible at first. You can either reveal the "Scripts Manager" tab from the "Tools" menu item or select _"Reset Window Layout"_ from the "Window" menu. We suggest the latter as the positioning of the Scripts Manager will default to a less than ideal position.

## Install the example Scripts
Please note that the example scripts are only available in the download version.

From the Scripts Manager click the "Restore Example Scripts" button. Example jArchi scripts will be copied to the Scripts Manager in an "examples" folder.

![restore_examples](https://user-images.githubusercontent.com/600504/57181605-ea307880-6e8d-11e9-90ef-05a16b10e2db.png)

You can run each script by double-clicking it or clicking the "Run Script" button.

_Note that some scripts assume that you have an ArchiMate model selected in the Models Tree or a View._

jArchi scripts have an *.ajs file extension.

A good start is to select a model in the Models Tree and run the "Anonymize" script. Take a look at the model's names, documentation and properties. Click "Undo" in Archi to undo this action.

## Select your Script Editor
As jArchi scripts are JavaScript you should set your preferred code editor in Archi's Preferences. This is found in the "Scripting" tab of Preferences. Set the path to your preferred editor. An example is "C:\Program Files\Microsoft VS Code\Code.exe". You can edit the script by pressing the "Edit" button.

## Running a Script
To run a script you can either press the "Run Script" button in the Scripts Manager or select the script from the "Scripts" context menu (right-click) available in the Models Tree or in a diagram view.

## Organise your Scripts
You can add new folders and files in the Scripts Manager as well as organising them with drag and drop. You can drag and drop files from the desktop to the Scripts Manager with the option to either copy them or link to them.

## Scripts Manager Options
### Refresh UI When Running Script
This is a menu item under the local menu (the little triangle) in the Scripts Manager toolbar. Normally, when long running scripts are run the UI is not updated and, if you are writing to console.log("message"), you won't see the console update until the script has finished. Enabling this allows the UI to be periodically updated in the background and so messages are written to the console in real time.

NOTE - As the whole of the Archi UI is updated as the script is run, the script will run slower. Only enable this if you want to see console.log() messages in real-time on long-running scripts.

![option1](https://user-images.githubusercontent.com/600504/57181727-316b3900-6e8f-11e9-9cfe-a8d89356298f.png)

## The Scripts Console
Script output messages and error messages can be seen in the Scripts Console. The Scripts Console can be toggled on or off by pressing the "Show Console" button in the Scripts Manager.

## The Current Model ("model")
jArchi works on the "Current Model". This is the selected model in the Models Tree or a View. For example, the following snippet adds a new Business Actor to the currently selected model:

`model.createElement("business-actor", "foo");`

You reference the current model as the `model` global variable.

## The Current Selection
You can reference currently selected objects in the Models Tree or View by using the selection global variable, `$(selection)` or `selection`.

For example, if you select some elements in the Models Tree and run the following script, all (valid) elements will be set to the Business Role type, " (changed)" appended to the name, and the fill color set to red.

```
$(selection).each(function(element) {
    element.type = "business-role";
    element.name += " (changed)";
    $(element).objectRefs().attr("fillColor", "#ff0000");
});
```