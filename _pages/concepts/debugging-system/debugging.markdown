---
layout: page
title: "The Debugging System"
category: concepts
order: 13
---

Although the Diagram SDK editor is not aware of execution, it provides a debugging system with a set of base classes, helpers, visual assets, and MEF contracts to support debugging.

## Debugging System Architecture

The following diagram illustrates the high-level architecture of the debugging system:

![DebuggingRelationships]

## Debugging System Theory of Operation

The debugging system consists of components for modeling, visualizing, customizing, and interacting with [Probes][ProbeRef] and [Breakpoints][BreakpointRef].

## Modeling Probes and Breakpoints

The [Probe][ProbeRef] and [Breakpoint][BreakpointRef] classes are [UserSetting][UserSettingRef] base classes available in the Diagram SDK, designed to support the aggregation and management of different types of probes and breakpoints (e.g. on a node vs. in text code) via the [ProbeManager][ProbeManagerRef] and [BreakpointManager][BreakpointManagerRef] classes. These manager classes in turn are used by clients, such as the [BreakpointWindowViewModel][BreakpointWindowViewModelRef], [ProbeWindowViewModel][ProbeWindowViewModelRef] and [DebuggingToolViewModel][DebuggingToolViewModelRef].

### Visualizing Probes and Breakpoints

The Diagram SDK supplies the following default visual controls for debugging:

* [BreakpointProbeControl][BreakpointProbeControlRef]
* [ExecutionPointControl][ExecutionPointControlRef]
* [BreakpointWindowControl][BreakpointWindowControlRef]
* [ProbeWindowControl][ProbeWindowControlRef]

The Diagram SDK also provides a set of default images, primarily used by DebuggingCommands for debugging-related buttons.

### Customizing Probes and Breakpoints

Probe and Breakpoint can be subclassed to store additional information about location/configuration. A [NodeViewModel][NodeViewModelRef] or a [DocumentEditControl][DocumentEditControlRef] can implement [IDebuggablePresentation][IDebuggablePresentationRef] to provide specific visuals for breakpoints. The vast majority of the commands in [DebuggingCommands][DebuggingCommandsRef] are [RoutedCommands][MSDN_RoutedCommands] so that the source (ViewModel or DocumentEditControl) can override to provide specific CanExecute() logic.

The Diagram SDK supplies the [IDataView][IDataViewRef] and [IDataViewFactory][IDataViewFactoryRef] interfaces as the plug-in mechanism to provide custom visualizations for data. For example, VI probes and Data Depot currently use this mechanism. Currently, all specific views are provided by VI. In the future, a set of standard data views might be pushed down to MocCommon or PlatformFramework.

### Interacting With Probes and Breakpoints

The DebuggingTool, accessed via [DebuggingToolViewModel][DebuggingToolViewModelRef], is a [DesignerTool][DesignerToolRef] that provides typical debugging interactions, such as panning when executing, clicking on a wire to show a probe, and displaying debugging adorners for probes and breakpoints. It also has built-in support for wire highlighting and disabled visualization for nodes that have not been executed.


[BreakpointRef]: http://xgen.amer.corp.natinst.com/DiagramSDK/html/e615e335-59e3-1e23-2f11-f507b32d049a.htm
[BreakpointManagerRef]: http://xgen.amer.corp.natinst.com/DiagramSDK/html/9d31b1ab-01ce-64b5-0f1a-9354bd726bf4.htm
[BreakpointProbeControlRef]: http://xgen.amer.corp.natinst.com/DiagramSDK/html/208331e3-40da-242a-ea23-7eb70686e3da.htm
[BreakpointWindowControlRef]: http://xgen.amer.corp.natinst.com/DiagramSDK/html/a1544712-4095-8c46-2c78-cff31c11b519.htm
[BreakpointWindowViewModelRef]: http://xgen.amer.corp.natinst.com/DiagramSDK/html/d99a173b-9479-02a6-f566-0372761d7992.htm
[DebuggingCommandsRef]: http://xgen.amer.corp.natinst.com/DiagramSDK/html/665ced8a-44dc-e0ed-72f1-a9d759271237.htm
[DebuggingToolViewModelRef]: http://xgen.amer.corp.natinst.com/DiagramSDK/html/df970654-082d-bf71-d574-48c609b09ef8.htm
[DesignerToolRef]: http://xgen.amer.corp.natinst.com/DiagramSDK/html/b9138942-a7e8-9d05-0e54-c5c1a90a9390.htm	
[DocumentEditControlRef]: http://xgen.amer.corp.natinst.com/DiagramSDK/html/41e6184c-7e62-e8f7-ea4c-eb2a7a676a43.htm
[ExecutionPointControlRef]: http://xgen.amer.corp.natinst.com/DiagramSDK/html/fce73701-a124-c748-3b92-82188dc0e2d5.htm
[IDataViewRef]: http://xgen.amer.corp.natinst.com/DiagramSDK/html/9ad2839f-575e-2d64-edfd-06172f1e237a.htm
[IDataViewFactoryRef]: http://xgen.amer.corp.natinst.com/DiagramSDK/html/2b002a83-cd1d-400f-e6dd-331bbe9294de.htm
[IDebuggablePresentationRef]: http://xgen.amer.corp.natinst.com/DiagramSDK/html/58fbe38c-93f5-9f28-000c-a41583996185.htm
[NodeViewModelRef]: http://xgen.amer.corp.natinst.com/DiagramSDK/html/570ebb43-70ca-3e3d-031d-88bd8715a39d.htm
[ProbeRef]: http://xgen.amer.corp.natinst.com/DiagramSDK/html/5a3e77fc-f5ec-474f-42dd-baf11fd1fdd7.htm
[ProbeManagerRef]: http://xgen.amer.corp.natinst.com/DiagramSDK/html/0f26689c-f7ea-06af-38a8-1511efe3b97d.htm
[ProbeWindowControlRef]: http://xgen.amer.corp.natinst.com/DiagramSDK/html/1a10ad7a-163e-fde5-2c62-811c6da28d0b.htm
[ProbeWindowViewModelRef]: http://xgen.amer.corp.natinst.com/DiagramSDK/html/03581e4f-0cfc-3e3a-4899-100c3d5ee697.htm
[UserSettingRef]: http://xgen.amer.corp.natinst.com/DiagramSDK/html/4ac52bc6-3f1b-b9bc-9122-2a1f93f65b89.htm

[MSDN_RoutedCommands]: http://msdn.microsoft.com/en-us/library/system.windows.input.routedcommand(v=vs.110).aspx


[DebuggingRelationships]: DebuggingRelationships2.png