## Table of contents
* [The Console](#the-console)
* [Prompt, Alert and File Dialogs](#prompt-alert-and-file-dialogs)
* [Including Other Script Files](#including-other-script-files)

---
# The Console
Output can be sent to jArchi's Console window using the following functions:

```js
console.show();
console.hide();
console.setText("Clear previous text and display this text");
console.log("Hello World");
console.log(object);
console.log("One thing: ", object, another, ...);
console.error(error);
console.clear();
console.setTextColor(redValue, greenValue, blueValue);
console.setDefaultTextColor();
```

# Prompt, Alert and File Dialogs

## Alert
```js
window.alert("Hello World");
```

## Confirm
```js
var response = window.confirm("Are you sure?");
```

## Prompt
```js
var name = window.prompt("Please enter your name", "Default Name");
```

## Prompt to open a file
```js
var filePath = window.promptOpenFile({ title: "Open Model", filterExtensions: [ "*.archimate" ], "default.archimate" });
```

## Prompt to open a directory
```js
var dirPath = window.promptOpenDirectory({ title: "Open Folder", "/defaultPath" });
```

## Prompt to save a file
```js
var filePath = window.promptSaveFile({ title: "Save Model", filterExtensions: [ "*.archimate" ], "default.archimate" });
```


# Including Other Script Files

You can include other Archi script or JavaScript files:

```js
load(absolute_path)
load(url)
```

There is no support for relative locations, so the following format is used:

```js
load(__DIR__ + "path/file.js")
```
```
__DIR__ contains the absolute path of the directory containing the current script.
__FILE__ contains the absolute path of current script.
__LINE__ contains the line number in which __LINE__ is used.
```