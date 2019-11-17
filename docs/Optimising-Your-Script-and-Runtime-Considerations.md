## Overview

Because there is an overhead when converting JavaScript to Java and back again, jArchi scripts will inevitably be slower than native code. Also, if the script is quite complex it can run for a long time and Archi may become unresponsive.

However, there are some things that can be done to alleviate the problem.

## Scripting

- Avoid writing to the console too many times. Too many `console.log()`, `console.print()`, and `console.println()` commands can slow things down, especially if these are in a looped operation that is repeated many times. In fact, closing the Console will speed things up if you write to it many times.


## Archi Setup

The new experimental feature, "Refresh UI When Running Script", is designed to allow the Scripts Console to update on long running operations. Archi's UI will also update whenever a jArchi command is issued. However, it also means that the Models Tree and any open Views will update as well which could slow things down.

- Ensure that all nodes in the Models Tree are collapsed. If you are updating many concepts (name, for example), each time you do this the Models Tree will refresh. Having many nodes expanded will slow things down.

- Ensure that all Views (ArchiMate diagrams, Sketch Views, Canvas Views) are closed. If you are updating many concepts (name, for example), each time you do this the View may refresh. Having many Views open will slow things down.