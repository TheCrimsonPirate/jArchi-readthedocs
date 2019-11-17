The following functions work on the `$.model` global variable.

## Table of contents
* [.create()](#create)
* [.load()](#load)
* [.isAllowedRelationship()](#isallowedrelationship)
* [.renderViewAsBase64()](#renderviewasbase64)

---

## .create()

Create a new model:

```js
var newModel = $.model.create("Test Model");
newModel.setAsCurrent(); // Set it to be the current model ("model")
```


## .load()

Load a model from file:

```js
var myModel = $.model.load("/path/test.archimate");
myModel.setAsCurrent(); // Set it to be the current model ("model")
model.openInUI(); // Open it in the UI (Models Tree)
```

## .isAllowedRelationship()

Return true if a relationship type is allowed between two concepts

```js
isAllowedRelationship(relationship-type, source-concept-type, target-concept-type)
```
Example:

```js
var isAllowed = isAllowedRelationship("assignment-relationship", "business-actor", "business-role")
```

## .renderViewAsBase64()
Get the image data encoded as Base64 from a View.

```js
.renderViewAsBase64(view, format, options...)
```

- view - reference to a view
- format - one of "PNG", "BMP", "JPG" or "GIF"
- options
  - scale = integer value of 1 to 4
  - margin = integer value of pixels


```js
var view = ...; // A view reference
var bytes = $.model.renderViewAsBase64(view, "PNG"); // Get bytes
// Embed in a HTML string
var html = "<html><body><p>" + view.name + "</p>" + "<img src=\"data:image/png;base64," + bytes + "\"></body></html>";
```

```js
var view = ...; // A view reference
var bytes = $.model.renderViewAsBase64(view, "PNG", {scale: 1, margin: 20}); // Get bytes
// Write to file
$.fs.writeFile("path/view.png", bytes, "BASE64");
```
