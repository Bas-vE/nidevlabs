---
layout: page
title: "Diagram SDK Overview"
category: concepts
order: 1
---

The NI Diagram SDK is a collection of .NET base classes, interfaces, and APIs designed to help you build:

* Diagrammatic models of computation (MoCs)
* Graphical MoC editors

## Diagram SDK Architecture

The following diagram illustrates the high-level architecture of the NI Diagram SDK, which is based on the [Model-View-ViewModel][MVVM] architectural pattern.

![Architectural Overview][Architectural_Overview]

## Diagram SDK Theory of Operation

The Diagram SDK consists of three primary components:

1. The [shell] provides a graphical user interface framework for building graphical MoC editors. The shell includes the following sub-components:
* The [document system][document_system] manages, displays and edits the end user's source files.
* The [diagram editing system][diagram_editing_system] displays and edits source file documents as graphical diagrams.
2. The [diagram object model (DiOM)][DiOM] provides a hierarchy of object-oriented classes and interfaces for modeling diagrammatic MoCs. The DiOM includes the following sub-components:
* The [transaction system][transaction_system] manages changes to DiOM [Elements].
* The [name resolution system][name_resolution_system] allows diagrams to link to external resources, such as classes and methods defined in a separate assembly.
* The [persistence system][persistence_system] saves and loads your MoC editor's diagrams as XML documents.
* The [messaging system][messaging_system] collects messages about DiOM Elements, such as compiler errors and warnings.
* The [wiring system][wiring_system] manages wiring between nodes on a diagram.
3. The [plug-in system][plug-in_system] allows you to write classes and methods that plug into the shell and the DiOM.

Follow the Diagram SDK [tutorials] to learn more about using the Diagram SDK to create a simple graphical MoC editor.

[MVVM]: ../InProgress.html
[shell]: ../InProgress.html
[document_system]: ../InProgress.html
[diagram_editing_system]: ../InProgress.html
[DiOM]: ../InProgress.html
[transaction_system]: ../InProgress.html
[Elements]: ../InProgress.html
[name_resolution_system]: ../InProgress.html
[persistence_system]: ../InProgress.html
[messaging_system]: ../InProgress.html
[wiring_system]: ../InProgress.html
[plug-in_system]: ../InProgress.html
[tutorials]: ../InProgress.html	

[Architectural_Overview]: DiagramSDKRelationships.png