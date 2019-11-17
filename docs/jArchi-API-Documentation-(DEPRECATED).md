# THIS PAGE IS MARKED AS DEPRECATED AS CONTENT HAS BEEN MOVED TO OTHER PAGES

>jArchi is the API used inside the Archi script plugin to access and manipulate ArchiMate models. See [[this page|jArchi-API-Overview]] for an overview.
>Some script snippets assume that you run them on [Archisurance.archimate](https://raw.githubusercontent.com/archimatetool/ArchiModels/a9ea4bdf8b7f2fd853fc63f66a045eddaf0fc9b3/Archisurance/Archisurance.archimate).
>In this documentation, "object" is used for any type  of information (ArchiMate concept or view, but also Canvas, Folders...).


## Table of contents
* [jArchi Collection](#jarchi-collection)
  * [Selectors](#selectors)
  * [Methods](#methods)

    |  Traversing                 | Navigation                   | Filtering            | Attributes &<br> Properties | Model      | Utils                |
    | --------------------------- | ---------------------------- | -------------------- | ---------------------------- | - | ---------------------- |
    | [.find()](#find)            | [.rels()](#rels)             | [.filter()](#filter) | [.attr()](#attr)             |   | [.each()](#each)       |
    | [.children()](#children)    | [.inRels()](#inrels)         | [.not()](#not)       | [.prop()](#prop)             |   | [.clone()](#clone)     |
    | [.parent()](#parent)        | [.outRels()](#outrels)       | [.has()](#has)       | [.removeProp()](#removeprop) |   | [.first()](#first)     |
    | [.parents()](#parents)      | [.ends()](#ends)             | [.add()](#add)       |                              |   | [.get()](#get)         |
    | [.viewRefs()](#viewrefs)    | [.sourceEnds()](#sourceends) |                      |                              |   | [.size()](#size)       |
    | [.objectRefs()](#objectrefs)| [.targetEnds()](#targetends) |                      |                              |   | [.is()](#is)           |

* [jArchi Object](#jarchi-object)
  * [Methods](#methods-1)

    | All Objects                        | Relationships &<br> Connections | Visual Objects           |
    | ---------------------------------- | ------------------------------- | ------------------------ |
    | [.id](#id)                         | [.source](#source)              | [.bounds](#bounds)       |
    | [.type](#type)                     | [.target](#target)              | [.fillColor](#fillcolor) |
    | [.name](#name)                     |                                 | [.fontColor](#fontcolor) |
    | [.documentation](#documentation)   |                                 | [.lineColor](#linecolor) |
    | [.prop()](#prop-1)                 |                                 | [.lineWidth](#linewidth) |
    | [.removeProp()](#removeprop-1)     |                                 | [.fontSize](#fontsize)   |
    |                                    |                                 | [.fontName](#fontname)   |
    |                                    |                                 | [.fontStyle](#fontstyle) |
    |                                    |                                 | [.concept](#concept)     |

* [Prompt, Alert and File Dialogs](#prompt-alert-and-file-dialogs)

* [Creating and deleting components](#creating-and-deleting-components)

* [Including other script files](#including-other-script-files)

---
# jArchi Collection
## Selectors
#### $(), jArchi()
This is the main selector method for jArchi. It returns an actionable collection of objects.
```js
$(selector) // => collection
$(collection) // => self
```

#### Name Selector ("._name_")
Select all objects with the given name.
````js
$(".Phone") // => collection containing all objects having the name 'Phone'
````

#### Object Selector ("_type_")
Select all objects with the given type (see table below).
````js
$("business-object") // => collection containing all Business Objects
````

| Strategy         | Business               | Application               | Technology               | Physical             |
| ---------------- | ---------------------- | ------------------------- | ------------------------ | -------------------- |
| resource         | business-actor         | application-component     | node                     | equipment            |
| capability       | business-role          | application-collaboration | device                   | facility             |
| course-of-action | business-collaboration | application-interface     | system-software          | distribution-network |
|                  | business-interface     | application-function      | technology-collaboration | material             |
|                  | business-process       | application-process       | technology-interface     |                      |
|                  | business-function      | application-interaction   | path                     |                      |
|                  | business-interaction   | application-event         | communication-network    |                      |
|                  | business-event         | application-service       | technology-function      |                      |
|                  | business-service       | data-object               | technology-process       |                      |
|                  | business-object        |                           | technology-interaction   |                      |
|                  | contract               |                           | technology-event         |                      |
|                  | representation         |                           | technology-service       |                      |
|                  | product                |                           | artifact                 |                      |

| Motivation  | Implementation <br>& Migration | Other    | Relationships               | Other Visual Objects     | Views                    |
| ----------- | ------------------------------ | -------- | --------------------------- | ------------------------ | ------------------------ |
| stakeholder | work-package                   | location | composition-relationship    | diagram-model-note       | archimate-diagram-model |
| driver      | deliverable                    | grouping | aggregation-relationship    | diagram-model-group      | sketch-model             |
| assessment  | implementation-even            | junction | assignment-relationship     | diagram-model-connection | canvas-model             |
| goal        | plateau                        |          | realization-relationship    | diagram-model-image      |                          |
| outcome     | gap                            |          | serving-relationship        | diagram-model-reference  |                          |
| principle   |                                |          | access-relationship         | sketch-model-sticky      |                          |
| requirement |                                |          | influence-relationship      | sketch-model-actor       |                          |
| constraint  |                                |          | triggering-relationship     | canvas-model-block       |                          |
| meaning     |                                |          | flow-relationship           | canvas-model-sticky      |                          |
| value       |                                |          | specialization-relationship | canvas-model-image       |                          |
|             |                                |          | association-relationship    |                          |                          |


#### ID Selector ("#_id_")
Select a single element with the given id.
````js
$("#477") // => in Archisurance.archimate, 'Maintaining Customer Relations' Business Function
````

#### All Selector ("*")
Select all objects.
````js
$("*") // => in Archisurance.archimate, collection containing 313 objects
````

#### Concepts Selector ("concept")
Select all ArchiMate concepts.
````js
$("concept") // => collection
````

#### Elements Selector ("element")
Select all ArchiMate elements.
````js
$("element") // => collection
````

#### Relationships Selector ("relationship")
Select all ArchiMate relationships.
````js
$("relationship") // => collection
````

#### Views Selector ("view")
Select all views (ArchiMate, Sketch, Canvas).
````js
$("view") // => collection
````

#### Folders Selector ("folder")
Select all folders.
````js
$("folder") // => collection
````


## Methods
>These methods are available once you create a collection with `$(selector)` or `jArchi(selector)`.

### Traversing
>These methods will 'traverse' the model structure which is made of folders and views. All of them return a new collection and don't mutate the original one.
#### .find()
Get the descendants of each object in the set of matched objects, optionally filtered by a selector.
````js
collection.find() // => collection of all descendants
collection.find(selector) // => collection of descendants that match the selector
````

#### .children()
Get the children of each object in the set of matched objects, optionally filtered by a selector.
````js
collection.children() // => collection of all children
collection.children(selector) // => collection of children that match the selector
````

#### .parent()
Get the parent of each object in the set of matched objects, optionally filtered by a selector.
````js
collection.parent() // => collection of parents
collection.parent(selector) // => collection of parents that match the selector
````

#### .parents()
Get the ancestors of each object in the set of matched objects, optionally filtered by a selector.
````js
collection.parents() // => collection of ancestors
collection.parents(selector) // => collection of ancestors that match the selector
````

#### .viewRefs()
Get the views in which each object in collection is referenced, optionally filtered by a selector.
````js
collection.viewRefs() // => collection of views
collection.viewRefs(selector) // => collection of views that match the selector
````

#### .objectRefs()
Get the visual objects that reference each object in collection, optionally filtered by a selector.
````js
collection.objectRefs() // => collection of (visual) objects
collection.objectRefs(selector) // => collection of (visual) objects that match the selector
````


### Navigation
>These methods will 'navigate' the model through relationships. All of them return a new collection and don't mutate the original one.
#### .rels()
Get the relationships that start or end at each concept in the set of matched objects, optionally filtered by a selector.
````js
collection.rels() // => collection of relationships
collection.rels(selector) // => collection of relationships that match the selector
````

#### .inRels()
Get the (incoming) relationships that end at each concept in the set of matched objects, optionally filtered by a selector.
````js
collection.inRels() // => collection of relationships
collection.inRels(selector) // => collection of relationships that match the selector
````

#### .outRels()
Get the (outgoing) relationships that start at each concept in the set of matched objects, optionally filtered by a selector.
````js
collection.outRels() // => collection of relationships
collection.outRels(selector) // => collection of relationships that match the selector
````

#### .ends()
Get the concepts that are the targets or the sources of each relationship in the set of matched objects, optionally filtered by a selector.
````js
collection.ends() // => collection of concepts
collection.ends(selector) // => collection of concepts that match the selector
````

#### .sourceEnds()
Get the concepts that are the sources of each relationship in the set of matched objects, optionally filtered by a selector.
````js
collection.sourceEnds() // => collection of concepts
collection.sourceEnds(selector) // => collection of concepts that match the selector
````

#### .targetEnds()
Get the concepts that are the targets of each relationship in the set of matched objects, optionally filtered by a selector.
````js
collection.targetEnds() // => collection of concepts
collection.targetEnds(selector) // => collection of concepts that match the selector
````

### Filtering
>These methods allow to add to, or remove objects from the collection (which is mutated).
#### .filter()
Reduce the set of matched elements to those that match the selector or pass the function's test.
````js
collection.filter(selector) // => collection of objects that match the selector
collection.filter(function(object) {return someBoolean}) // collection of objects that pass the test
````

#### .not()
Remove elements from the set of matched elements.
````js
collection.not(selector) // => collection of concepts that don't match the selector
````

#### .has()
Reduce the set of matched elements to those that have a descendant that matches the selector.
````js
collection.has(selector) // => collection of concepts
````

#### .add()
Create a new jArchi Collection with objects added to the set of matched objects. Added objects can come from another collection of be determined through a selector.
````js
collection.add(selector) //=> collection
collection.add(otherCollection) // => collection
````


### Attributes & Properties
#### .attr()
Get the value of an attribute for the first object in the set of matched object or set one or more attributes for every matched object.
````js
collection.attr(attrName) // => AttributeValue
collection.attr(attrName, attrValue) // => updated collection
````

#### .prop()
If no arguments are provided, return the list of properties' key for the first object in the collection.
Return a property value of the first object in the collection when just property is supplied. If multiple properties exist with the same key, then return only the first one (duplicate=false, which is the default) or an array with all values (duplicate=true)
Sets a property for every objects when property and value are supplied. Property is updated if it already exists (duplicate=false, which is the default) or added anyway (duplicate=true).
````js
collection.prop() //=> properties' key for the first object
collection.prop(propName) // => property value (first one if multiple entries)
collection.prop(propName, false) // => property value (first one if multiple entries)
collection.prop(propName, true) // => property value (array if multiple entries)
collection.prop(propName, propValue) // => updated collection with property added (if it did not exist) or updated (if it existed)
collection.prop(propName, propValue, false) // => updated collection with property added (if it not existed) or updated (if it existed)
collection.prop(propName, propValue, true) // => updated collection with property added (even if it existed)
````

#### .removeProp()
Removes property from collection objects.
````js
collection.removeProp(propName) // => all instances of propName are removed
collection.removeProp(propName, propValue) // => properties are removed if value matches propValue
````

### Model

### Utils
#### .each()
Iterate over a collection, executing a function for each object. The function to execute will receive the current object as first argument.
````js
collection.each(function(obj) {console.log(obj.id)}) // => print ids of all objects
````

#### .clone()
Create a copy of the set of matched objects. Objects themselves are not copied, only the collection is.
````js
collection.clone() // => a copy of the collection
````

#### .first()
Reduce the set of matched objects to the first in the set.
````js
collection.first() // => first object
````

#### .get()
Retrieve one of the objects matched by the collection.
````js
collection.get(n) // => return object at index 'n' from the collection
````

#### .size()
Return the number of objects in the collection.
````js
$('business-object').size() // => Return the number of Business Object in the current model
````

#### .is()
Check the current matched set of object against a selector and return true if at least one of these objects matches the given arguments.
````js
$(selection).is('business-object') // => Return true is there is at least one Business Object in the selection, false otherwise
````

# jArchi Object
## Methods
### All Objects
>Attributes and methods described in this section are available on all jArchi Objects instances

#### .id
Get object's id. This attribute is read-only.
````js
object.id // => AttributeValue
````

#### .type
Get object's type (internal EMF class name). This attribute is writable for ArchiMate concepts: changing its value will replace the concept itself and update all references. If setting the concept type would result in an illegal relationship, an exception is thrown.
````js
object.type // => Object's type
object.type = 'business-process' // => change the nature of the ArchiMate concept to 'Business Process'
````

#### .name
Get/set object's name.
````js
object.name // => AttributeValue
object.name = 'New name'
````

#### .documentation
Get/set object's documentation.
````js
object.documentation // => AttributeValue
object.documentation = 'New documentation content'
````

#### .prop()
If no arguments are provided, return the list of properties' key for the object.
Return a property value from the object when just property is supplied. If multiple properties exist with the same key, then return only the first one (duplicate=false, which is the default) or an array with all values (duplicate=true)
Sets a property on the object when property and value are supplied. Property is updated if it already exists (duplicate=false, which is the default) or added anyway (duplicate=true).
````js
object.prop() //=> properties' key for the first object
object.prop(propName) // => property value (first one if multiple entries)
object.prop(propName, false) // => property value (first one if multiple entries)
object.prop(propName, true) // => property value (array if multiple entries)
object.prop(propName, propValue) // => updated object with property added (if it did not exist) or updated (if it existed)
object.prop(propName, propValue, false) // => updated object with property added (if it not existed) or updated (if it existed)
object.prop(propName, propValue, true) // => updated object with property added (even if it existed)
````

#### .removeProp()
Removes property from an objects.
````js
object.removeProp(propName) // => all instances of propName are removed
object.removeProp(propName, propValue) // => properties are removed if value matches propValue
````

### Relationships & Connections
>Attributes and methods described in this section are available only on ArchiMate Relationships and Diagram Connections

#### .source
Get/set the source of an ArchiMate Relationship.
````js
object.source // => return the concept which is the source of the relationship
object.source = someJArchiObject // => set the source using a jArchi Object
object.source = $('#someid') // => set the source using a jArchi Collection (first member will be used)
````
Note - setting the source of a relationship currently only works if the relationship is not present on a view, otherwise an exception is thrown. A future update will allow this.

#### .target
Get/set the target of an ArchiMate Relationship.
````js
object.target // => return the concept which is the target of the relationship
object.target = someJArchiObject // => set the target using a jArchi Object
object.target = $('#someid') // => set the target using a jArchi Collection (first member will be used)
````
Note - setting the target of a relationship currently only works if the relationship is not present on a view, otherwise an exception is thrown. A future update will allow this.

### Visual Objects
>Attributes and methods described in this section are available only on [[Visual Objects|jArchi-API-Overview#anatomy-of-a-model-in-archi]] (i.e., boxes and connections that appear on a diagram and inherit from DiagramModelObject).
>Colors are coded as hex triplet (red, green, blue) with a leading hash. For example red is coded as "#ff0000".

#### .bounds
Get/set the bounds of a visual object (box). Setting width and height to -1 sets default width and height.

#### .fillColor
Get/set the fill color. Does nothing on connections. Setting to null sets as user's default color.

#### .fontColor
Get/set the font color.

#### .lineColor
Get/set the line color of a visual object (box or connection). If preference for line color derived from fill color is set, has no effect.

#### .lineWidth
Get/set the connection line width. Has no effect on objects (boxes).

#### .fontSize
Get/set the font size (height);

#### .fontName
Get/set the font name.

#### .fontStyle
Get/set the font style (normal, bold, italic, bolditalic).

#### .concept
Get the ArchiMate concept related to a visual object (box or connection).
````js
object.concept // => given that object is a Diagram Model Object, return the related ArchiMate concept
connection.concept // => given that connection is a Diagram Model Connection, return the related ArchiMate concept
````


# Prompt, Alert and File Dialogs

## Alert
````js
window.alert("Hello World");
````

## Confirm
````js
var response = window.confirm("Are you sure?");
````

## Prompt
````js
var name = window.prompt("Please enter your name", "Default Name");
````

## Prompt to open a file
````js
var filePath = window.promptOpenFile({ title: "Open Model", filterExtensions: [ "*.archimate" ], "default.archimate" });
````

## Prompt to open a directory
````js
var dirPath = window.promptOpenDirectory({ title: "Open Folder", "/defaultPath" });
````

## Prompt to save a file
````js
var filePath = window.promptSaveFile({ title: "Save Model", filterExtensions: [ "*.archimate" ], "default.archimate" });
````

# Creating and deleting components

## Creating elements and junctions
````js
var actor = model.addElement("business-actor", "Oscar");
var junction = model.addElement("junction", "");
````

## Creating relationships
````js
var actor = model.addElement("business-actor", "Oscar");
var role = model.addElement("business-role", "Cat");
var relationship = model.addRelationship("assignment-relationship", "Assigned to", actor, role);
````

## Deleting components

Elements, Relationships, Folders, and Views and their child diagram objects and connections can be deleted:

````js
element.delete();
relationship.delete();
folder.delete();
view.delete();
diagramElement.delete();
diagramConnection.delete();
````



# Including other script files

You can include other Archi script or JavaScript files:

````js
load(absolute_path)
load(url)
````

There is no support for relative locations, so the following format is used:

````js
load(__DIR__ + "path/file.js")
````
```
__DIR__ contains the absolute path of the directory containing the current script.
__FILE__ contains the absolute path of current script.
__LINE__ contains the line number in which __LINE__ is used.
```