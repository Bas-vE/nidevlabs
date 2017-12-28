---
template: page
title: "The Document System"
category: concepts
order: 4
---

The Diagram SDK document system is responsible for managing, displaying and editing the user's source files within a [shell application][Shell].

## Document System Architecture

The following diagram illustrates the high-level architecture of the document system.

![DocSystemRelationships]

## Document System Theory of Operation

When the end user opens a [shell application][Shell], the [ShellApplication][ShellApplicationRef] instantiates a [DocumentManager][DocumentManagerRef], which is responsible for managing the life cycle of all [Documents][DocumentsRef] opened within the application. The ShellApplication's [StudioWindow][StudioWindowRef] instantiates a [DocumentEditor][DocumentEditorRef], which is responsible for managing the DocumentEditControls][DocumentEditControlRef] that edit Documents.

When the user opens an XML source file from disk, the [persistence system][PersistenceSystem] parses the file to create a [SourceFile][SourceFileRef], which represents the underlying XML source file in memory. The DocumentEditor instantiates the default DocumentEditControl specified by the Document's [DocumentType][DocumentTypeRef]. The DocumentEditControl provides a view that the end user can interact with to edit the document.

## Primary Classes that Participate in the Document System

### SourceFile

A [SourceFile][SourceFileRef] represents a persisted file as a hierarchy of Diagram Object Model [Elements][ElementsRef] in memory. A Document serves as a [ViewModel][MVVM] for a SourceFile.

### IDocumentType

The [IDocumentType][IDocumentTypeRef] interface must be implemented by all [DocumentType][DocumentTypeRef] plug-ins. To be recognized by a shell application, you must export your DocumentTypes through the [plug-in system][Plugins] by declaring them as implementers of the IDocumentType contract. DocumentTypes are primarily responsible for creating new Documents and opening existing SourceFiles as Documents.

### Document

A Document is a ViewModel for a [SourceFile][SourceFileRef]. The purpose of a Document is to provide editor features for a SourceFile. Documents are created by their corresponding [ocumentType][DocumentTypeRef] factory classes and are used to track what is currently being edited. Documents have two major purposes:

1. Providing commands and handlers for a SourceFile
2. Connecting a SourceFile's Elements to DocumentEditControls in the View layer

### DocumentManager

The [DocumentManager][DocumentManagerRef] maintains a list of currently open Documents and provides mechanisms to bind a SourceFile to a Document. It owns the lifetimes of Documents. The DocumentManager is a singleton for an application instance. The DocumentManager also owns the reference to the current project. There is always one and only one project open at a time for a given shell application instance. There is also a requirement that everything opened is part of the current project. The DocumentManager implements the [IDocumentManager][IDocumentManagerRef] interface, which is importable through the [plug-in framework][Plugins].

### DocumentType

The [DocumentType][DocumentTypeRef] class is a factory used to create Documents bound to SourceFiles.

### DocumentEditor

[DocumentEditor][DocumentEditorRef] is the root control in the [shell][Shell] responsible for document editing. The DocumentEditor is an internal object whose role is to:

* Instantiate [DocumentEditControls][DocumentEditControlRef] for open documents and provide the user with the means of switching between available DocumentEditControls
* Provide helpers for DocumentEditControls to navigate document hierarchies
* Provide zoom and auto-zoom behavior

A document editor comprises a hierarchy of .NET FrameworkElements. At the root of the hierarchy is the DocumentEditControl, which includes a ResourceDictionary and a DesignerEditControl. The resource dictionary contains DataTemplates that can be data bound to model elements. The DesignerEditControl contains a RootDiagramCanvas, which is responsible for binding the model elements to the data templates.

### DocumentEditSite

The [DocumentEditSite][DocumentEditSiteRef] type provides services to user interface elements of a DocumentEditControl, including:

* Access to the application instance and the [CompositionContainer][CompositionContainerRef]
* Methods to edit related documents
* Methods to change the active editor for a document

You can call [DocumentEditSite.GetEditSite][GetEditSiteRef] to access the DocumentEditSite for a specific dependency object.

### DocumentEditControl

The [DocumentEditControl][DocumentEditControlRef] type provides the root visual element for editing a Document. Thus, a DocumentEditControl is a View for a SourceFile. DocumentEditControls are hosted in the StudioWindow[StudioWindowRef] Documents tab. Each IDocumentType can support one or more DocumentEditControls. For example, a VI supports both a front panel DocumentEditControl and a block diagram DocumentEditControl.

The DocumentEditControl includes a [ResourceDictionary][MSDN_ResourceDictionary] and a [DesignerEditControl][DesignerEditControlRef]. The ResourceDictionary contains [DataTemplates][MSDN_DataTemplates] that can be data bound to model [Elements][ElementsRef].

DocumentEditControl is the first plug-in point in the visual tree, and serves as a View of a SourceFile. A DocumentEditControl can control everything it shows the user. However, a DocumentEditControl for a diagram-based Document typically uses a nested [DesignerEditControl][DesignerEditControlRef] to take advantage of the Diagram SDK's built-in [diagram editing system][DiagramEditingSystem]. In this case, the DocumentEditControl would:

1. Include a nested DesignerEditControl in its XAML definition.
2. Include a nested RootDiagram within its DesignerEditControl.
3. Override the [Designer][DesignerRef] property in the code-behind to return the nested DesignerEditControl.
4. Override the [CreateDefaultTool][CreateDefaultToolRef] method to return a default [AutoTool][AutoToolRef] with a set of custom sub-tools. See [Tool Management][ToolManagement] for more details.
5. Override the [CreateRibbonBars][CreateRibbonBarsRef] method to return top-level Ribbon tab content to display when the DocumentEditControl is active.

[DiagramEditingSystem]: ..\InProgress.html
[MVVM]: ..\InProgress.html
[PersistenceSystem]: ..\InProgress.html
[Plugins]: ..\plugin-system\plugins.html
[Shell]: ..\shell\shell.html
[ToolManagement]: ..\InProgress

[AutoToolRef]: http://xgen.amer.corp.natinst.com/DiagramSDK/html/T_NationalInstruments_SourceModel_Designer_AutoTool.htm
[CompositionContainerRef]: http://msdn.microsoft.com/en-us/library/system.componentmodel.composition.hosting.compositioncontainer.aspx
[CreateDefaultToolRef]: http://xgen.amer.corp.natinst.com/DiagramSDK/html/M_NationalInstruments_SourceModel_Shell_DocumentEditControl_CreateDefaultTool.htm
[CreateRibbonBarsRef]: http://xgen.amer.corp.natinst.com/DiagramSDK/html/M_NationalInstruments_SourceModel_Shell_DocumentEditControl_CreateRibbonBars.htm
[DesignerRef]: http://xgen.amer.corp.natinst.com/DiagramSDK/html/P_NationalInstruments_SourceModel_Shell_DocumentEditControl_Designer.htm
[DesignerEditControlRef]: http://xgen.amer.corp.natinst.com/DiagramSDK/html/T_NationalInstruments_SourceModel_Designer_DesignerEditControl.htm
[DocumentEditControlRef]: http://xgen.amer.corp.natinst.com/DiagramSDK/html/T_NationalInstruments_SourceModel_Shell_DocumentEditControl.htm
[DocumentEditSiteRef]: http://xgen.amer.corp.natinst.com/DiagramSDK/html/T_NationalInstruments_SourceModel_Shell_DocumentEditSite.htm
[DocumentEditorRef]: http://xgen.amer.corp.natinst.com/DiagramSDK/html/T_NationalInstruments_Shell_DocumentEditor.htm
[DocumentManagerRef]: http://xgen.amer.corp.natinst.com/DiagramSDK/html/T_NationalInstruments_Shell_DocumentManager.htm
[DocumentTypeRef]: http://xgen.amer.corp.natinst.com/DiagramSDK/html/T_NationalInstruments_SourceModel_Shell_DocumentType_1.htm
[DocumentsRef]: http://xgen.amer.corp.natinst.com/DiagramSDK/html/T_NationalInstruments_SourceModel_Shell_Document.htm
[ElementsRef]: http://xgen.amer.corp.natinst.com/DiagramSDK/html/T_NationalInstruments_SourceModel_Modeling_Element.htm
[GetEditSiteRef]: http://xgen.amer.corp.natinst.com/DiagramSDK/html/M_NationalInstruments_SourceModel_Shell_DocumentEditSite_GetEditSite.htm
[IDocumentManagerRef]: http://xgen.amer.corp.natinst.com/DiagramSDK/html/T_NationalInstruments_SourceModel_Shell_DocumentType_1.htm
[IDocumentTypeRef]: http://xgen.amer.corp.natinst.com/DiagramSDK/html/T_NationalInstruments_SourceModel_Shell_IDocumentType.htm
[ShellApplicationRef]: http://xgen.amer.corp.natinst.com/DiagramSDK/html/T_NationalInstruments_Shell_Restricted_ShellApplication.htm
[SourceFileRef]: http://xgen.amer.corp.natinst.com/DiagramSDK/html/T_NationalInstruments_SourceModel_Modeling_SourceFile.htm	
[StudioWindowRef]: http://xgen.amer.corp.natinst.com/DiagramSDK/html/T_NationalInstruments_Shell_StudioWindow.htm

[MSDN_DataTemplates]: http://msdn.microsoft.com/en-us/library/system.windows.datatemplate.aspx
[MSDN_ResourceDictionary]: http://msdn.microsoft.com/en-us/library/system.windows.resourcedictionary.aspx

[DocSystemRelationships]: DocSystemRelationships.png