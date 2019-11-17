# Table of contents

### All Objects
[.id](#id)  
[.type](#type)  
[.name](#name)  
[.documentation](#documentation)  
[.prop()](#prop)  
[.removeProp()](#removeprop)  
[.delete()](#delete)  
[.model](#model)  

### Model
[.purpose](#purpose)  
[.copy()](#copy)  
[.createElement()](#createelement)  
[.createRelationship()](#createrelationship)  
[.createArchimateView()](#createarchimateview)  
[.createSketchView()](#createsketchview)  
[.createCanvasView()](#createcanvasview)  
[.getPath()](#getpath)  
[.openInUI()](#openinui)  
[.save()](#save)  
[.setAsCurrent()](#setascurrent)  

### Elements
[.merge](#merge)  

### Relationships
[.source](#source)  
[.target](#target)  
[.accessType](#accesstype)  
[.influenceStrength](#influencestrength)  

### Folder
[.add()](#add)  
[.createFolder()](#createfolder)  

### View (Diagram)
[.add(element)](#addelement)  
[.add(relationship)](#addrelationship)  
[.createObject()](#createobject)  
[.viewpoint](#viewpoint)  
[.isAllowedConceptForViewpoint()](#isAllowedConceptForViewpoint)  

### Visual Objects
[.add(element)](#addelement2)  
[.createObject()](#createobject2)  
[.bounds](#bounds)  
[.opacity](#opacity)  
[.fillColor](#fillcolor)  
[.fontColor](#fontcolor)  
[.lineColor](#linecolor)  
[.fontSize](#fontsize)  
[.fontName](#fontname)  
[.fontStyle](#fontstyle)  
[.concept](#concept)  
[.view](#view)  
[.text](#text)  
[.figureType](#figureType)  

### Visual Connections
[.source](#source2)  
[.target](#target2)  
[.lineWidth](#linewidth)  
[.relativeBendpoints](#relativebendpoints)  
[.addRelativeBendpoint()](#addrelativebendpoint)  
[.deleteAllBendpoints()](#deleteallbendpoints)  
[.deleteBendpoint()](#deletebendpoint)  



---

## All Objects
>Attributes and methods described in this section are available on all jArchi Object instances

#### .id 
Get object's id. This attribute is read-only.
```js
object.id // => AttributeValue
```

Example:
```js
var conceptID = businessActor.id;  // id of a concept
var objectID = diagramObject.id; // id of a visual diagram object
var conceptID = diagramObject.concept.id; // id of the concept referenced by a visual diagram object
```

#### .type
Get object's type (see [[jArchi Collection]] for a list of types). This attribute is writeable for ArchiMate concepts: changing its value will replace the concept itself and update all references. If setting the concept type would result in an illegal relationship, an exception is thrown.
```js
object.type // => Object's type
object.type = 'business-process' // => change the nature of the ArchiMate concept to 'Business Process'
```

#### .name
Get/set object's name.
```js
object.name // => AttributeValue
object.name = 'New name'
```

#### .documentation
Get/set object's documentation.
```js
object.documentation // => AttributeValue
object.documentation = 'New documentation content'
```

#### .prop()
If no arguments are provided, return the list of properties' key for the object.
Return a property value from the object when just property is supplied. If multiple properties exist with the same key, then return only the first one (duplicate=false, which is the default) or an array with all values (duplicate=true)
Sets a property on the object when property and value are supplied. Property is updated if it already exists (duplicate=false, which is the default) or added anyway (duplicate=true).
```js
object.prop() //=> properties' key for the first object
object.prop(propName) // => property value (first one if multiple entries)
object.prop(propName, false) // => property value (first one if multiple entries)
object.prop(propName, true) // => property value (array if multiple entries)
object.prop(propName, propValue) // => updated object with property added (if it did not exist) or updated (if it existed)
object.prop(propName, propValue, false) // => updated object with property added (if it not existed) or updated (if it existed)
object.prop(propName, propValue, true) // => updated object with property added (even if it existed)
```

#### .removeProp()
Removes property from an objects.
```js
object.removeProp(propName) // => all instances of propName are removed
object.removeProp(propName, propValue) // => properties are removed if value matches propValue
```

#### .delete()
Deletes an object.
```js
customer.delete() // => A Business Actor with variable name, `customer`, is removed from the model
folder.delete() // => A folder, with variable name, `folder`, is removed from the model, as well as its contents
```

Note - a model cannot be deleted in this way.
Note - an object that is a diagram connection cannot be deleted from the model in this way (it will just remove it from the diagram). To remove a diagram selected `object` you need to use .concept like this:
```
obj.concept.delete();
```

See also [[Creating and Deleting Objects]]

#### .model
Returns the container model for an object.
```js
var model = customer.model // => Returns the container model for `customer` object.
var model = folder.model // => Returns the container model for `folder` object.
```

## Model
>Attributes and methods described in this section are available on the model object

#### .purpose
Get/set model's purpose (documentation).
```js
var purpose = object.purpose;
object.purpose= 'New purpose content';
```

#### .copy()
Returns a new reference to the model. Example:
```js
var modelref = model.copy(); // => Copy a reference to the current model
```

#### .createElement()
Create and add a new element to its default folder in the model.

````js
model.createElement(element-type, name);
````

Create and add a new element to a given folder in the model:

````js
model.createElement(element-type, name, folder);
````

Use the kebab-case type of the element and provide a name. Examples:

````js
var actor = model.createElement("business-actor", "Oscar");
var role = model.createElement("business-role", "Cat");
var node = model.createElement("node", "Node");
var junction = model.createElement("junction", "");
````

#### .createRelationship()
Create and add a new relationship to its default folder in the model.

````js
model.createRelationship(relationship-type, name, source, target);
````

Create and add a new relationship to a given folder in the model:

````js
model.createRelationship(relationship-type, name, source, target, folder);
````

Use the kebab-case type of the relationship and provide a name (can be "") and the source and target components. Example:

````js
var actor = model.createElement("business-actor", "Oscar");
var role = model.createElement("business-role", "Cat");
var relationship = model.createRelationship("assignment-relationship", "Assigned to", actor, role);
````

#### .createArchimateView()
Create a new ArchiMate View in the model.

Create and add a new view (to the default folder) in the model:

````js
model.createArchimateView(name);
````

Create and add a new view to a given folder in the model:

````js
model.createArchimateView(name, folder);
````

Examples:
````js
var view = model.createArchimateView("New Archimate View");
var view2 = model.createArchimateView("Archimate View", myFolder);
````

#### .createSketchView()
Create a new Sketch View in the model.

Create and add a new view (to the default folder) in the model:

````js
model.createSketchView(name);
````

Create and add a new view to a given folder in the model:

````js
model.createSketchView(name, folder);
````

Examples:
````js
var view = model.createSketchView("New Sketch View");
var view2 = model.createSketchView("Sketch View", myFolder);
````

#### .createCanvasView()
Create a new Canvas View in the model.

Create and add a new view (to the default folder) in the model:

````js
model.createCanvasView(name);
````

Create and add a new view to a given folder in the model:

````js
model.createCanvasView(name, folder);
````

Examples:
````js
var view = model.createCanvasView("New Canvas View");
var view2 = model.createCanvasView("Canvas View", myFolder);
````

#### .getPath()
Return the path to the file location of the model, or null if not saved.
```js
var filePath = model.getPath();
```

#### .openInUI()
Open the model in the UI (Models Tree).
```js
model.openInUI();
```

#### .save()
Save the model to file.
```js
model.save(); // If the model has previously been saved with a path and file name, save the model
model.save("path/test.archimate"); // Save the model to the given path and file name
```

#### .setAsCurrent()
Set a model to the current model. The current model is referenced as the global variable `model`.

Example:
```js
$.model.load("path/test.archimate").setAsCurrent(); // Load a model and set it as the current model
```

or:

```js
var myModel = model.load("path/test.archimate");
myModel.setAsCurrent();
```

### Elements
>Attributes and methods described in this section are available only on ArchiMate Elements

#### .merge()
Merge this element and another element.

```js
element.merge(otherElement);
```

- Existing diagram instances of the other Archimate element will be replaced with this element
- Elements have to be of the same type
- Documentatation of the other element is appended to this element's documentation
- Properites of the other element are appended to this element's properties
- All source and target relationships of the other element are set to this element
- The other element is not deleted

### Relationships
>Attributes and methods described in this section are available only on ArchiMate Relationships

#### .source
Get/set the source of an ArchiMate Relationship.
```js
relation.source // => return the concept which is the source of the relationship
relation.source = aConcept // => set the source using a Concept
```

#### .target
Get/set the target of an ArchiMate Relationship.
```js
relation.target // => return the concept which is the target of the relationship
relation.target = aConcept // => set the target using a Concept 
```

#### .accessType
Applies only to Access Relationships.

Get/set the access type of an Access Relationship.

Allowed types - "access", "read", "write", "readwrite"
```js
// Getters
var type = relation.getAccessType();
var type = relation.accessType;
var type = selection.attr("access-type");

// Setters
relation.setAccessType("write");
relation.accessType = "access";
selection.attr("access-type", "readwrite");
```

#### .influenceStrength
Applies only to Influence Relationships.

Get/set the strength of an Influence Relationship.

Any string is allowed.
```js
// Getters
var strength = relation.getInfluenceStrength();
var strength = relation.influenceStrength;
var strength = selection.attr("influence-strength");

// Setters
relation.setInfluenceStrength("++");
relation.influenceStrength = "++";
selection.attr("influence-strength", "+++");
```

## Folder
>Attributes and methods described in this section are available only on Folders in the Models Tree

#### .add()
Add a concept, a view or a sub-folder to a folder. If the object already has a parent folder then the object is moved to the folder.

```js
folder.add(concept);
folder.add(view);
folder.add(folder);
```

Example:

```js
var concept = ....; Reference to an existing concept
var folder = $(".folder.My Folder").first(); // Find folder called "My Folder" 
folder.add(concept); // Move the concept to this folder
```

#### .createFolder()
Create a new (sub-)folder.

```js
folder.createFolder("New Folder");
```

Example:

```js
var folder = $(".folder.Business").first(); // Find folder called "Business" 
folder.createFolder("Processes"); // Create a sub-folder called Processes
```

## View (Diagram)
>Methods described in this section are are available only on Views - ArchiMate, Sketch and Canvas Views.

#### .createObject()
Create and return a new visual object of type with given bounds.

`type` can be one of "note" or "group"

`autoNest` optional boolean parameter. If true the diagram object will be nested inside of any existing diagram object whose bounds surrounds the new object's bounds.

Setting width or height to -1 uses the default width and height.

```js
createObject(type, x, y, width, height)
createObject(type, x, y, width, height, autoNest) 
```

Example:

```js
var note = archimateView.createObject("note", 10, 200, 200, 100, true);
note.setText("This is a note.\n\nHello World!");

var group = archimateView.createObject("group", 10, 200, -1, -1);
```

#### .add(element)
Create and return a new diagram object referencing an ArchiMate element with given bounds.

This method only applies to ArchiMate Views.

`element` is an existing ArchiMate element.

`autoNest` optional boolean parameter. If true the diagram object will be nested inside of any existing diagram object whose bounds surrounds the new object's bounds.

Setting width or height to -1 uses the default width and height.

```js
add(element, x, y, width, height)
add(element, x, y, width, height, autoNest)
```

Example:

```js
var object= archimateView.add(businessActor, 10, 200, 150, 75);
var object= archimateView.add(businessRole, 10, 200, -1, -1, true);
```

#### .add(relationship)
Create and return a new visual object referencing an ArchiMate relationship.

`relationship` is an existing ArchiMate relationship.

```js
add(relationship, sourceDiagramComponent, targetDiagramComponent)
```

Example:

```js
var object1 = archimateView.add(actor, 10, 10, 150, 70);
var object2 = archimateView.add(role, 200, 10, 150, 70);
var connection = archimateView.add(relationship, object1, object2);
```

#### .viewpoint
Get/set the Viewpoint of an ArchiMate View.

Allowed viewpoint identifier strings that can be set:

```
application_cooperation
application_usage
business_process_cooperation
capability
goal_realization
implementation_deployment
implementation_migration
information_structure
layered
migration
motivation
organization
outcome_realization
physical
product
project
requirements_realization
resource
service_realization
stakeholder
strategy
technology
technology_usage
```

Setting the viewpoint identifier to the empty string "" sets it to no viewpoint.

The returned viewpoint JS object consists of the viewpoint ID and the human readable name.

Example:

```js
archimateView.viewpoint = "implementation_deployment";
var vp = archimateView.viewpoint;
var id = vp.id;
var name = vp.name;
```

#### .isAllowedConceptForViewpoint()

```js
archimateView.isAllowedConceptForViewpoint(conceptName)
```

Return true if the given concept is allowed in the current Viewpoint of an ArchiMate View.

Example:

```js
var isallowed = archimateView.isAllowedConceptForViewpoint("business-actor"));
```


## Visual Objects
>Attributes and methods described in this section are available only on [[Visual Objects|jArchi-API-Overview#anatomy-of-a-model-in-archi]] (i.e., boxes and connections that appear on a diagram and inherit from DiagramModelObject and DiagramModelConnection).

>Colors are coded as hex triplet (red, green, blue) with a leading hash. For example red is coded as "#ff0000".

#### <a id="addelement2"></a> .add(element)
Create and return a new nested visual object referencing an ArchiMate element with given bounds.

`element` is an existing ArchiMate element.

Setting width or height to -1 uses the default width and height.

```js
add(element, x, y, width, height) 
```

Example:

```js
var object= groupObject.add(businessActor, 10, 200, -1, -1);
var object= groupObject.add(businessRole, 10, 200, 50, 70);
```

#### <a id="createobject2"></a> .createObject()
Create and return a new visual object of type with given bounds.

`type` can be one of "note" or "group"

Setting width or height to -1 uses the default width and height.

```js
createObject(type, x, y, width, height)
```

Example:

```js
var group = archimateView.createObject("group", 10, 200, -1, -1); // Create top-level Group
var note = group.createObject("note", 10, 10, 150, 70); // Add a Note in this Group
```

#### .bounds
Get/set the bounds of a visual object (box). Setting width and height to -1 sets default width and height. x, y, width and height are optional

```js
diagramObject.bounds = {x: 10, y: 10, width: 120, height: 55};
diagramObject.bounds = {x: 10, y: 10, width: -1, height: -1};
diagramObject.bounds = {x: 10, y: 10};
```

#### .opacity
Get/set the opacity of a visual object (box). Does nothing on connections. Value range 0-255.

```js
diagramObject.opacity = 100; // Sets opacity to half
```

#### .fillColor
Get/set the fill color. Does nothing on connections. Setting to null sets as user's default color.

```js
diagramObject.fillColor = "#ff0000"; // Sets fill color to red
diagramObject.fillColor = "#00ff00"; // Sets fill color to green
diagramObject.fillColor = null; // Sets fill color to default color.
```

#### .fontColor
Get/set the font color. A null value sets to default.

```js
diagramObject.fontColor = "#ff0000"; // Sets font color to red
diagramObject.fontColor = "#00ff00"; // Sets font color to green
diagramObject.fontColor = null; // Sets font color to default color.
```

#### .lineColor
Get/set the line color of a visual object (box or connection). If preference for line color derived from fill color is set, this has no effect. A null value sets to default.

```js
diagramObject.lineColor = "#ff0000"; // Sets line color to red
diagramObject.lineColor = "#00ff00"; // Sets line color to green
diagramObject.lineColor = null; // Sets line color to default color.
```

#### .fontSize
Get/set the font size (height).

```js
diagramObject.fontSize = 12;
```

#### .fontName
Get/set the font name.

```js
diagramObject.fontName= "Arial";
```

#### .fontStyle
Get/set the font style (normal, bold, italic, bolditalic).

```js
diagramObject.fontStyle = "normal";
diagramObject.fontStyle = "bold";
diagramObject.fontStyle = "italic";
diagramObject.fontStyle = "bolditalic";
```

#### .concept
Get the ArchiMate concept related to a visual object (box or connection).
```js
var concept = object.concept; // => given that object is a View Object, return the related ArchiMate concept
var concept = connection.concept; // => given that connection is a View Connection, return the related ArchiMate concept
```

#### .view
Get the view that contains this visual object (box or connection).
```js
var view = object.view; // => given that object is a View Object, return the containing View
var view = connection.view; // => given that connection is a View Connection, return the containing View
```

#### .text
Get/set the text for a Note object.
```js
var note = archimateView.createObject("note", 10, 200, 150, 200);
note.setText("This is a note.");
console.log(note.text);
```

#### .figureType
Get/set the figure type for of a visual object (box). This applies to ArchiMate element diagram objects that support two different figure types.

Permitted values are 0 and 1.
```js
var type = object.figureType();
var type = object.figureType; 
object.figureType = 1;
object.setFigureType(0);
```

## Visual Connections
>Attributes and methods described in this section are available only on Diagram Connections

#### <a id="source2"></a> .source
Get the source of a Connection.
```js
connection.source // => return the diagram object/connection which is the source of the connection
```

#### <a id="target2"></a> .target
Get the target of a Connection.
```js
connection.target // => return the diagram object/connection which is the target of the connection
```

#### .lineWidth
Get/set the connection line width.

```js
connection.lineWidth = 1; // Sets line width to normal
connection.lineWidth = 2; // Sets line width to medium
connection.lineWidth = 3; // Sets line width to heavy
```

#### .relativeBendpoints

Get the relative bendpoint positions of a connection.

Returns an array list of bendpoints in this format:

```
endY:
endX:
startY:
startX:
```

```js
connection.relativeBendpoints
```

Example:

```js
var bendpoints = connection.getRelativeBendpoints();
var bendpoints = connection.relativeBendpoints;
```

#### .addRelativeBendpoint()

Add a bendpoint to a relative position on a connection at index position.

```js
connection.addRelativeBendpoint(bendpoint, index)
```

Example:

```js
// New bendpoint 1
var bp1 = {
    startX: 200,
    startY: 200,
    endX: 10,
    endY: 10
}

// New bendpoint 2
var bp2 = {
    startX: 100,
    startY: 100,
    endX: 20,
    endY: 30
}

// Select the first connection in an opened View
var connection = selection.filter("relationship").first();

// Add bendpoints at index positions
connection.addRelativeBendpoint(bp1, 0);
connection.addRelativeBendpoint(bp2, 1);
```

#### .deleteAllBendpoints()

Delete all bendpoints on a connection.

```js
connection.deleteAllBendpoints()
```

#### .deleteBendpoint()

Delete a bendpoint at index position on a connection.

```js
connection.deleteBendpoint(index)
```

Throws an `ArchiScriptException` if index position is out of bounds.
