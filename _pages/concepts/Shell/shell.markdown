---
layout: page
title: "The Shell"
category: concepts
order: 2
---

The Shell serves as the foundation of your application's graphical user interface and manages relationships between projects, documents, and user interface elements.

## Shell Architecture

The following diagram illustrates the high-level architecture of the Shell.

![ShellRelationships]

## Shell Theory of Operation

The Shell is the foundation of the Diagram SDK's graphical user interface infrastructure. It is designed to serve as an [extensible][plug-in_framework], user-friendly basis for your application's graphical user interface. The Shell provides both a rich, configurable application window full of built-in features and a powerful set of [controls][ShellControlsRef] and a robust set of [APIs][ShellRef] upon which to build your own custom GUI.

### Built-In ShellApplication Features
[ShellApplication][ShellApplicationRef] base type to serve as the basis for your main application window. When you start developing your application, your will begin by inheriting from ShellApplication, which provides basic features such as:

* [StudioWindow][StudioWindowRef]
* [DocumentManager][DocumentManagerRef]
* [Quick Access Toolbar][QuickAccessToolbarRef]
* [Application Menu][ApplicationMenuRef]
* [Ribbon][RibbonRef]
* [DocumentWindow][DocumentWindowRef]
* Plug-in Tool Window
* Property Editor
* StudioWindow Status Bar
* Hierarchy Bar
* Document View Selector
* Zoom Control

### ShellApplication Visual Tree Hierarchy

The following diagram shows the Shell components in context and illustrates how the Shell's [visual tree hierarchy][VisualTreeHierarchyDocs] maps to the visuals you see in the StudioWindow[StudioWindowRef], from DocumentWindow down to [DiagramCanvas][DiagramCanvasRef]. Note that some of the types in the visual tree hierarchy are invisible.

![ShellUIHierarcy]

### Abstract Structure of the ShellApplication

The following diagram presents a simplified, abstract view that highlights how the Shell's primary components fit together.

![ShellStructure]

## Primary Shell API Types

The following classes form the backbone of the Shell API. However, these types represent only a only a small fraction of the full API. For more information about the Shell API, see the [NationalInstruments.Shell][ShellRef] namespace documentation. For more information about the Shell View Controls, see the [NationalInstruments.ShellControls][ShellControlsRef] namespace documentation.

### ShellApplication

The [ShellApplication][ShellApplicationRef] is a base type from which you can derive your own application. ShellApplication instantiates a StudioWindow to host the application's graphical user interface.

### StudioWindow

The [StudioWindow][StudioWindowRef] is the root visual object of the Shell. The StudioWindow is a [Window][MSDN_WindowRef] on WPF and a [UserControl][MSDN_UserControlRef] on Silverlight. The StudioWindow creates the StudioWindowEditSite and implements the functionality that it provides.

### DocumentManager

The [DocumentManager][DocumentManagerRef] tracks open [Documents][DocumentRef]. A Document encapsulates a Model and provides commands and a [DocumentEditControl][DocumentEditControlRef] to manipulate the model.

### Ribbon

The [Ribbon][RibbonRef] is the swath of GUI Controls that occupies the top section of the StudioWindow. The Ribbon is the primary means of exposing configuration options and commands for the ShellApplication, Document, and RootDiagram.

### RibbonTab

A [RibbonTab][RibbonTabRef] is a collection of RibbonGroups that displays as a selectable tab in the Ribbon.

### RibbonGroup

The [RibbonGroup][RibbonGroupRef] class allows you to define a collection of Ribbon controls to add to a RibbonTab.

### IPushRibbonContent

[IPushRibbonContent][IPushRibbonContentRef] is an interface you can implement to provide custom RibbonGroups and Commands to the default RibbonTabs.

### IApplicationFeatureSet

[IApplicationFeatureSet][IApplicationFeatureSetRef] is an interface you can implement to customize your ShellApplication.


[ApplicationMenuRef]: http://xgen.amer.corp.natinst.com/DiagramSDK/html/T_NationalInstruments_Shell_StudioRibbonApplicationMenu.htm
[DiagramCanvasRef]: http://xgen.amer.corp.natinst.com/DiagramSDK/html/T_NationalInstruments_SourceModel_Designer_DiagramCanvas.htm
[DocumentRef]: http://xgen.amer.corp.natinst.com/DiagramSDK/html/T_NationalInstruments_SourceModel_Shell_Document.htm
[DocumentEditControlRef]: http://xgen.amer.corp.natinst.com/DiagramSDK/html/T_NationalInstruments_SourceModel_Shell_DocumentEditControl.htm
[DocumentManagerRef]: http://xgen.amer.corp.natinst.com/DiagramSDK/html/T_NationalInstruments_Shell_DocumentManager.htm
[IApplicationFeatureSetRef]: http://xgen.amer.corp.natinst.com/DiagramSDK/html/T_NationalInstruments_SourceModel_Common_IApplicationFeatureSet.htm
[IPushRibbonContentRef]: http://xgen.amer.corp.natinst.com/DiagramSDK/html/T_NationalInstruments_SourceModel_Shell_IPushRibbonContent.htm		
[QuickAccessToolbarRef]: http://xgen.amer.corp.natinst.com/DiagramSDK/html/T_NationalInstruments_Shell_StudioRibbonQuickAccessToolBar.htm
[RibbonRef]: http://xgen.amer.corp.natinst.com/DiagramSDK/html/T_NationalInstruments_ShellControls_Ribbon_Ribbon.htm
[RibbonGroupRef]: http://xgen.amer.corp.natinst.com/DiagramSDK/html/T_NationalInstruments_ShellControls_Ribbon_RibbonGroup.htm
[RibbonTabRef]: http://xgen.amer.corp.natinst.com/DiagramSDK/html/T_NationalInstruments_ShellControls_Ribbon_RibbonTab.htm
[ShellApplicationRef]: http://xgen.amer.corp.natinst.com/DiagramSDK/html/T_NationalInstruments_Shell_Restricted_ShellApplication.htm
[ShellControlsRef]: http://xgen.amer.corp.natinst.com/DiagramSDK/html/N_NationalInstruments_ShellControls.htm
[ShellRef]: http://xgen.amer.corp.natinst.com/DiagramSDK/html/N_NationalInstruments_Shell.htm
[StudioWindowRef]: http://xgen.amer.corp.natinst.com/DiagramSDK/html/T_NationalInstruments_Shell_StudioWindow.htm
[VisualTreeHierarchyDocs]: http://msdn.microsoft.com/en-us/library/ms753391.aspx#two_trees

[ShellRelationships]: ShellRelationships.png
[ShellStructure]: ShellStructure.png
[ShellUIHierarcy]: ShellUIHierarchy.png

[DocumentWindowRef]: ..\InProgress.html
[plug-in_framework]: ..\InProgress.html


[MSDN_WindowRef]: http://msdn.microsoft.com/en-us/library/system.windows.window.aspx
[MSDN_UserControlRef]: http://msdn.microsoft.com/en-us/library/system.windows.forms.usercontrol.aspx

