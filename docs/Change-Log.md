### Version 0.6.0 - October 20 2019
- Add API to merge concepts
- Fix setting a Note's content not using the Command Stack
- When setting new concept type diagram objects use figure type as set in Preferences
- Allow to set and get ArchiMate Diagram Object figure type
- FolderProxy#add() supports adding Concepts, Views and Folders
- Check integrity of model when saving
- Fix "class.name" selector for name containing dots
- Add ArchimateModelProxy#children to allow access to first level folders
- Some ES6 support (experimental)

### Version 0.5.0 - July 12 2019
- Add autoNest boolean option to ArchimateDiagramModelProxy.add(element, x, y, width, height, autoNest)
- Add autoNest boolean option to DiagramModelProxy.createObject(type, x, y, width, height, autoNest)
- Add Add Relative Bendpoints API to Connections
- Allow selector of id # on model and add diagram object selectors
- Improve EObjectProxyCollection methods' speed by using a LinkedHashSet
- Ensure that source and target diagram components belong to the correct parent diagram model
- Compare file names in menu and file viewer so that they are in alphabetical order on Mac
- Improve the speed of script Undo/Redo if the Models Tree is open

### Version 0.4.5 - April 16 2019
- Fix setOpacity() not put on Undo/Redo Command Stack (regression in 0.4.4)
- Set the current active part on startup if available
- CurrentModel throws exception if model is null in UI (this is better than a NPE)
- Console View moves Caret to end of text


### Version 0.4.4 - March 22 2019
- Add getPath() to ArchimateModelProxy
- Add getters and setters for Access Type and Influence Strength
- [Console] Add Scroll Lock option and add new Toolbar icons
- Add option to refresh UI when running a script


### Version 0.4.3 - February 12 2019
- Don't show hidden files in Scripts Manager
- Fix regression of exported images x2 on hires screens on Windows


### Version 0.4.2 - February 01 2019
- find() no longer returns connected relationships on diagram components
- Don't scale exported images x2 on hires screens
- Fix exception on activating scripts menu if the user scripts directory does not exist


### Version 0.4.1 - October 26 2018
- Calling exit() now exits gracefully with a message to console log
- Add getOpacity() and setOpacity() to View Objects
- setBounds() now returns self
- Add setViewpoint(id) and getViewpoint() to ArchiMate Views
- Add isAllowedConceptForViewpoint(conceptName) to ArchiMate Views


### Version 0.4.0 - August 9 2018
- Add new examples to premium content
- Add $.model.isAllowedRelationship(relationshipType, conceptType, conceptType)
- Add createObject(type, x, y, width, height) function to View and View Objects
- Add add(element, x, y, width, height) function to View and View Objects
- Add add(relation, sourceDiagramComponent, targetDiagramComponent) to View
- concept.setType(type) now copies documentation


### Version 0.3.0 - July 23 2018
- Add new examples to premium content
- Add model.createArchimateView(name)
- Add model.createSketchView(name)
- Add model.createCanvasView(name)
- getType() now returns kebab-case class name
- Relationship setSource() and setTarget() now updates all view instances of connections
- Rename model.addElement() and model.addRelationship() to model.createElement() and model.createRelationship()
- Added folder.add(concept) and folder.createFolder(name)
- Rename DiagramComponent#getDiagramModel() to DiagramComponent#getView()
- Console.log() can print JavaScript map-type objects (example - "{x:10 y:20}")
- Use a map of options (scale, margin) for $.model.renderViewAsBase64(options)
- Show script name in Undo/Redo menu
- Scripts Manager sorts properly alphabetically on Linux


### Version 0.2.0 - July 12 2018

- Add new examples to premium content
- Rename getArchimateConcept() to getConcept()
- Add diagram object and connection get/set functions: fontColor, fontName, fontSize, fontStyle, lineColor, lineWidth, fillColor. These are now camel case and can be used in attr() functions.
- Add set/get bounds function to view objects and use a JS object for x, y, width, height
- Add $.model.renderViewAsBase64(view, format, options) function
- Simplify Browser object API
- Add options to prompt and file dialogs
- Fix drag and drop not working at top level in Scripts Manager


### Version 0.1.0 - July 2 2018

- First beta release