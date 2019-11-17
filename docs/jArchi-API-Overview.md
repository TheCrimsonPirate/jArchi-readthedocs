>Note: This page documents jArchi as we'd like it to be. Don't expect to have everything implemented yet ;-)

## Table of contents
* [Anatomy of a model in Archi](#anatomy-of-a-model-in-archi)
* [Two API](#two-api)
  * [Archi Internal API](#archi-internal-api)
  * [jArchi API](#jarchi-api)
  * [Modifying the model from jArchi API](#modifying-the-model-from-jarchi-api)
* [Available methods](#available-methods)
  * [Mutator and accessor methods](#mutator-and-accessor-methods)
  * [jArchi Collection methods](#jarchi-collection-methods)
  * [jArchi Object methods](#jarchi-object-methods)
* [What you can and can't do with jArchi](#what-you-can-and-cant-do-with-jarchi)
  * [Focus is on model content](#focus-is-on-model-content)
  * [Views as groups](#views-as-groups)

---
## Anatomy of a model in Archi
![archimodel](https://user-images.githubusercontent.com/5757396/40182518-e57ba268-59eb-11e8-9029-b6b43e9136e2.png)

In Archi, a model is composed of several parts:
* **ArchiMate model content** which contains all concepts (elements and relationships), automatically classified under "per layer" top-level folders, and then (optionally) user-defined sub-folders.
* **Diagrams** that can be ArchiMate related (ArchiMate views) or not (Canvas and Sketch). Diagrams are stored on the "Views" top-level folder, and then (optionally) user-defined sub-folders.
* **Diagram content** is made of visual objects that can contain other objects and that can be linked to ArchiMate concepts (if the diagram is an ArchiMate view).

**ArchiMate model content** (concepts and folders) and **Diagrams** (views and folders) share a common structure:
* They have a **name**
* They have a **description**
* They have a **list of properties**

>In addition Folders have **children** (a concept being in only one Folder), and relationship have **source** and **target**

**Visual objects** (from diagram content) are more complex:
* They **inherit** name, description and properties if they are linked to an **underlying ArchiMate concept**
* They have **their own** name, description and list of properties otherwise
* They have **visual properties** like: position, size, color, font...

>Visual nesting (putting an object inside another object) is another way to group objects. By analogy, we'll consider that an object can have **children**, but this is only meaningful in the context of a Diagram, not the whole model.


## Two API
The Archi scripting plugin allows you to write script using JavaScript and run them inside Archi (either through the GUI or using the command line). For those scripts to be useful, the ArchiMate model must be accessible through a set of objects, methods and functions. Those methods and functions are commonly refered to as an [API](https://en.wikipedia.org/wiki/Application_programming_interface) (Application Programming Interface).

### Archi Internal API
Archi already provides an API to manipulate the model. This API is based on [EMF](https://en.wikipedia.org/wiki/Eclipse_Modeling_Framework) (Eclipse Modelling Framework) and can be considered a low-level, internal API. There's absolutly nothing bad in using this API (and that's what Archi itself and all contributed plugins do), but this can be complex and error prone. In EMF, an object is called an EObject. In Archi, each and every classes inherit from EObject.

### jArchi API
To help people create powerful scripts quickly with no hassle, a higher level API has been designed. This API relies on the internal API but hides its complexity. This API is named jArchi. It has been inspired by [jQuery](http://jquery.com/), a very popular JavaScript library that does a similar job in simplifying usage of native [DOM API](https://en.wikipedia.org/wiki/Document_Object_Model) in browsers.

From within a script, jArchi API is used through the `jArchi()` function. The $ symbol is an alias for this function, so `$(...)` is equivalent to `jArchi(...)`. jArchi function can be referred to as a "wrapper" function since it effectively wraps stuff inside an instance of an EObjectProxyCollection class.

When you call jArchi() you're creating a unique instance of EObjectProxyCollection that can be referenced and accessed like any other object in JavaScript:
````js
var foo = $('business-object.MyVerySpecificObject'); // using '$' which is an alias for 'jArchi()'
foo.attr('description', 'Description of this business-object has been changed through a script.');
````

In effect, an EObjectProxyCollection is simply a collection (an array) of EObjectProxy. EObjectProxy is also a "wrapper" that contains a single EObject. This additional layer of abstraction is needed to simplify some common actions (like accessing properties) but also to make sure that a [specific behavior](#changing-model-from-jarchi-api) is triggered when updating some values.
````js
$('business-object').each(function(o) {
  console.log(o.prop('Data Owner'))
});
````

>From now on we'll use "jArchi Collection" as a synonym for EObjectProxyCollection, and "jArchi Object" as a synonym for EObjectProxy.


### Modifying the model from jArchi API
If you make changes to a model that is open in the UI, the Script action is undoable from Archi's Undo/Redo menu. Running a Script that does not perform any write operations will not incur an undoable action


## Available methods
### Mutator and accessor methods
Methods that apply on a jArchi Collection can be classified as:
* **Mutator methods**: A mutator method (a.k.a "modifier"/"setter"), in it's most general form, is a method that changes something within the instance.
* **Accessor methods**: An accessor method (a.k.a "getter") is a method that returns a value, without mutating (changing) anything.

In addition, some accessor methods return a jArchi Collection too, but it's not the same one, it's a new one. E.g. Calling .children() gives you a new jArchi Collection; it does not modify the current one.

Here are some accessor methods (with some example arguments):
* .children()
* .attr('name')
* .prop('key')

Here are some mutator methods (again with example arguments):
* .attr('name', 'Foo')
* .prop('key', 'value')
* .removeProp('key')

>Quite a few of jArchi's methods act as mutators and accessors, depending on what arguments you pass to them. For example, with one argument, the attr() method is an accessor, and with two it's a mutator.

### jArchi Collection methods
jArchi Collection methods focus on:
* Creating a collection: through a _selector_ (`$(selector)`) or _transversal_ methods (`children()`, `parent()`, `source()`...).
* Filtering a collection (`filter(selector)`...).
* Updating collection members: changing their attributes (`attr(name, value)`) or properties (`prop(key, value)`, `removeProp(key)`...).
* Accessing some attributes (`attr(name)`) or properties (`prop(key)`) of first collection member.

### jArchi Object methods
jArchi Object methods focus on:
* Accessing and updating attributes through a natural syntax: `someJArchiElement.someAttribute`.
* Accessing and updating properties (`prop(key)`, `prop(key, value)`, `removeProp(key)`...). Note that this is the exact same methods than for a jArchi Collection.

>Note: It is still unclear to me how deletions should be handled. Should we call a `delete()` method on jArchi Object or should we call a `remove(object)` method on `model`. The later would open the way to moving objects from one model to another.

## What you can and can't do with jArchi
### Focus is on model content
jArchi is governed by a simple principle: **Focus is on model content**. which means that jArchi is able to query and edit ArchiMate elements and relationship, their name, documentation and properties. In theory, working on the model content can be done using only jArchi.

_Printing a Data Dictionary:_
````js
$('business-object').each(function(o) {
  console.log(o.name, o.documentation);
});
````

_Getting the list of Application Service needed by a Business Process which is selected on the model tree:_
````js
// .filter().first() is to make sure we work on only one Business Process
// (in case of multiple or erroneous selection)
var bp = $(selection).filter('business-process').first(); 
console.log('Here is the list of services supporting ', bp.name);

$(bp).inRels('serving-relationship').each(function(r) {
    if(r.source.type === "application-service") {
        console.log(' - ', r.name);
    }
});
````

### Views as groups
An ArchiMate model in Archi is not only about elements and relationships, it is also about Views. jArchi focuses on a View's (visual) structure, not on it graphical representation. This means that a View is seen as a group of concepts (and some of these concepts can contains other concepts, through visual nesting). In this approach, Views are very similar to a model Folder.

jArchi will help you to query this structure to extract information or update underlying ArchiMate concepts. At the moment there is also limited access to the visual aspects such as fill colour, font, line width.