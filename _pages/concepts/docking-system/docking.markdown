---
layout: page
title: "The Docking System"
category: concepts
order: 15
---

The Diagram SDK docking system manages the docking of tool windows within a [shell application][Concept_shell] window and provides an API through which you can customize tool window appearance and docking behavior.

## Docking System Architecture

The following diagram illustrates the high-level architecture of the docking system.

![DockingRelationships]

## Docking System Theory of Operation

The Diagram SDK provides support for tool windows, which are semi-autonomous, yet subordinate to the main shell application window. Tool windows can either float freely or dock within the main application window, and you can customize their appearance and docking behaviors using the APIs described in this document.

### Creating a Custom Tool Window

Complete the following steps to create a custom tool window:

1. Create the visual for your window (e.g. a UserControl with a nested Grid that contains your tool window's UI elements.
2. Create a new class that derives from [ToolWindowType][ToolWindowTypeRef].
3. Export your ToolWindowType class as an [IToolWindowType][IToolWindowTypeRef].
4. Declare [ExportMetadataAttributes][MSDN_ExportMetadataAttribute] to customize your tool window. See the next section for more information about customization options.
5. Override the [IToolWindowType.CreateToolWindow()][CreateToolWindow] method and return an instance of your visual class.

The following example code demonstrates a simple ToolWindowType implementation.

```C#
    [Export(typeof(IToolWindowType))]
    [ExportMetadata("UniqueID", "{804b449c-a447-7c94-a4f8-d9317b9eba73}")]
    [Name("MyCustomToolWindow")]
    [ExportMetadata("SmallImagePath", "")]
    [ExportMetadata("LargeImagePath", "")]
    [ExportMetadata("DefaultCreationTime", ToolWindowCreationTime.UserRequested)]
    [ExportMetadata("DefaultCreationMode", ToolWindowCreationMode.Pinned)]
    [ExportMetadata("DefaultLocation", ToolWindowLocation.BottomRightDock)]
    [ExportMetadata("AssociatedDocumentType", ArchitecturalDocument.TypeName)]
    [ExportMetadata("Weight", 0.5)]
    [ExportMetadata("ForceOpenPinned", true)]
    public class MyToolWindowType : ToolWindowType
    {
        public override NationalInstruments.Core.PlatformVisual CreateToolWindow()
        {
            return new MyCustomToolWindow();
        }
    }
```

### Customizing Tool Window Docking Behavior

Tool window docking behavior customizations are exposed as properties on the [IToolWindowTypeInfo][IToolWindowTypeInfoRef] interface and are set through ExportMetadata declarations on your ToolWindowType. Common customizations include:

* Make a tool window float upon creation - set __FloatsOnCreation__ to true
* Remove a tool window's tab bar – set __ShowWindowTabBarWhileDocked__ and/or __ShowWindowTabBarWhileFloating__ to false
* Include standard window controls on a tool window – set __CanMaximizeRestoreAndClose__ to true (note, however, that tool windows cannot be minimized)

Refer to the the IToolWindowTypeInfo [IToolWindowTypeInfoRef] interface documentation for the complete list of options and their default values.

### Creating Custom Dock Harbors

The phrase _dock harbor_ refers to the spaces within the shell window that hold docked tool windows. By default, the only available dock harbors are the spaces that the shell application highlights when the user drags a tool window. If these default dock harbors are insufficient for your use case, you can define custom dock harbors for your tool window(s) by providing:

* An [IRestrictedDockHarborHost][IRestrictedDockHarborHostRef]
* An [IToolWindowType][IToolWindowTypeRef] for the tool window that sets __UsesRestrictedDockHarbor__ to true
* A visual to act as the dock harbor itself

The IRestrictedDockHarborHost interface provides methods to perform docking system customizations. The interface’s documentation explains where each of its methods fit in the scheme of the docking system’s execution logic. However, the following points shed additional light on the interface:

* You can use the [IRestrictedDockHarborHost.AllowedToolWindowIsDocking()][AllowedToolWindowIsDockingRef] method to display visual docking hints similar to those provided by default. The docking system calls this method whenever the user starts dragging a tool window whose Guid fits the host’s [AllowedToolWindowId][AllowedToolWindowIdRef], so this is a logical place to perform docking set-up processes in addition to displaying visual hints.
* When the user stops dragging a tool window, the docking operation for the tool window stops and the docking system calls one of two functions, depending on how the operation ended:
 * If the user releases the mouse in the visual returned by GetDockHarbor, the docking system calls the [IRestrictedDockHarborHost.Dock()][DockRef] method. It is the IRestrictedDockHarborHost's responsibility to facilitate the interaction between the dock harbor visual and the tool window.
 * If the user releases the mouse anywhere else, the docking system calls the [IRestrictedDockHarborHost.AllowedToolWindowFinishedDockingElsewhere()][AllowedToolWindowFinishedDockingElsewhereRef] method.
* Note that each custom dock harbor can be used with only one type of tool window.

### Persisting Docking Layouts

The term _docking layout_ refers to the complete configuration of all tool windows on the screen – which ones are open, where they are, whether they are docked, etc. By default, the docking system persists the user's docking layout in the application preferences. However, you can choose to persist the docking layout in the open project’s settings file instead by setting [ProjectSettings.PersistentLayoutRecordingRule][PersistentLayoutRecordingRuleRef] to __UpdateLayoutRecord__.

### Programmatically Accessing Docking States

[ToolWindowEditSite][ToolWindowEditSiteRef] provides programmatic access to the docking state of a particular tool window. You can access a tool window's edit site by passing its visual to the [ToolWindowEditSite.GetEditSite()][GetEditSite] method. External clients can call [DocumentEditSite.ShowToolWindow()][ShowToolWindowRef] to access the visual for a particular tool window. [ToolWindowEditSite][ToolWindowEditSiteRef] offers a number of useful properties and methods, including:

* [Show()][ShowRef] to open the tool window and make it visible
* [Hide()][HideRef] to close the tool window
* [Pinned][PinnedRef] to check whether the tool window is currently pinned, or to pin or unpin it




[Concept_shell]: ..\InProgress.html

[MSDN_ExportMetadataAttribute]: http://msdn.microsoft.com/en-us/library/system.componentmodel.composition.exportmetadataattribute(v=vs.110).aspx

[AllowedToolWindowFinishedDockingElsewhereRef]: http://xgen/DiagramSDK/html/M_Divelements_SandDock_IRestrictedDockHarborHost_AllowedToolWindowFinishedDockingElsewhere.htm
[AllowedToolWindowIdRef]: http://xgen.amer.corp.natinst.com/DiagramSDK/html/P_Divelements_SandDock_IRestrictedDockHarborHost_AllowedToolWindowId.htm
[AllowedToolWindowIsDockingRef]: http://xgen.amer.corp.natinst.com/DiagramSDK/html/M_Divelements_SandDock_IRestrictedDockHarborHost_AllowedToolWindowIsDocking.htm
[CreateToolWindow]: http://xgen.amer.corp.natinst.com/DiagramSDK/html/2d6a522b-326a-34e4-c159-6f1b31464a71.htm
[DockRef]: http://xgen/DiagramSDK/html/M_Divelements_SandDock_IRestrictedDockHarborHost_Dock.htm
[GetEditSite]: http://xgen.amer.corp.natinst.com/DiagramSDK/html/M_NationalInstruments_SourceModel_Shell_ToolWindowEditSite_GetEditSite.htm
[HideRef]: http://xgen.amer.corp.natinst.com/DiagramSDK/html/M_NationalInstruments_SourceModel_Shell_ToolWindowEditSite_Hide.htm
[IRestrictedDockHarborHostRef]: http://xgen.amer.corp.natinst.com/DiagramSDK/html/T_Divelements_SandDock_IRestrictedDockHarborHost.htm
[IToolWindowTypeRef]: http://xgen.amer.corp.natinst.com/DiagramSDK/html/37b92fe1-89f8-27de-055d-977e6ccb10fe.htm
[IToolWindowTypeInfoRef]: http://xgen.amer.corp.natinst.com/DiagramSDK/html/2f4d8470-aab3-fdcd-b95d-12488439d114.htm
[PersistentLayoutRecordingRuleRef]: http://xgen.amer.corp.natinst.com/DiagramSDK/html/151756ff-f15a-63a8-81a9-5bd3737c88da.htm
[PinnedRef]: http://xgen.amer.corp.natinst.com/DiagramSDK/html/P_NationalInstruments_SourceModel_Shell_ToolWindowEditSite_Pinned.htm	
[ShowRef]: http://xgen.amer.corp.natinst.com/DiagramSDK/html/M_NationalInstruments_SourceModel_Shell_ToolWindowEditSite_Show.htm
[ShowToolWindowRef]: http://xgen.amer.corp.natinst.com/DiagramSDK/html/dfc5ef6c-6036-07be-e3f4-27d2782a35f4.htm
[ToolWindowEditSiteRef]: http://xgen.amer.corp.natinst.com/DiagramSDK/html/AllMembers_T_NationalInstruments_SourceModel_Shell_ToolWindowEditSite.htm
[ToolWindowTypeRef]: http://xgen.amer.corp.natinst.com/DiagramSDK/html/06c1f416-6012-f0d7-4df8-db3b1a36c4ea.htm

	

[DockingRelationships]: DockingRelationships.png