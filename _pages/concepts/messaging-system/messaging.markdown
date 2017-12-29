---
layout: page
title: "The Messaging System"
category: concepts
order: 11
---

The Diagram SDK messaging system provides the ability to report messages of varying severity on any object in the [Diagram Object Model (DiOM)][Concept_DiOM]. You can use these message reporting features to attach transitive information to a model that can be reported back to the end user in various ways. Most commonly, the messaging system is used to report compilation errors and warnings.

## Messaging System Architecture

The following diagram illustrates the high-level architecture of the messaging system.

![MessagingRelationships]

## Messaging System Theory of Operation

The messaging system provides a framework for reporting, collecting, and querying [Messages][MessageRef]. The Element base class provides properties and methods [MessageScope][ElementMessageScopeRef] property that returns its [IMessageGroupScope][IMessageGroupScopeRef]. The IMessageGroupScope walks the owner hierarchy aggregating messages. An element can query its MessageScope for its own messages as well as its descendants' massages. Thus, all Diagram SDK model types participate in the messaging system by default. You can can send messages to any element from anywhere in the Model or ViewModel layer. To display messages to the end user, you can create a ViewModel that pulls messages from the Msa changes and maintains a public collection of current messages in the Model layer. You then can create a view that binds to this collection to display messages to the end user.

## Primary Types that Participate in the Messaging System

The following types form the backbone of the messaging system.

### Message

A [Message][MessageRef] contains information about the state of an Element that may be of interest to the end user. Messages are most commonly used to report errors and warnings. The [Message Constructor][MessageConstructorRef] allows you to pass in several types of message information, including reporting type, severity, category, code, and formatting arguments.

### IMessageGroupScope

[IMessageGroupScope][IMessageGroupScopeRef] is an interface implemented by [SourceFile][SourceFileRef] to aggregate all the messages reported on Elements contained within the SourceFile.

### Element

Messages are reported to specific model [Elements][ElementRef]. The Element base class includes [properties and methods][ElementPropertiesAndMethodsRef] you can use to report and query messages on an Element.



[Concept_DiOM]: ../InProgress.html	

[ElementRef]: http://xgen.amer.corp.natinst.com/DiagramSDK/html/T_NationalInstruments_SourceModel_Modeling_Element.htm
[ElementMessageScopeRef]: http://xgen.amer.corp.natinst.com/DiagramSDK/html/P_NationalInstruments_SourceModel_Modeling_Element_MessageScope.htm
[ElementPropertiesAndMethodsRef]: http://xgen.amer.corp.natinst.com/DiagramSDK/html/AllMembers_T_NationalInstruments_SourceModel_Modeling_Element.htm
[IMessageGroupScopeRef]: http://xgen.amer.corp.natinst.com/DiagramSDK/html/T_NationalInstruments_SourceModel_Modeling_IMessageGroupScope.htm
[MessageRef]: http://xgen.amer.corp.natinst.com/DiagramSDK/html/T_NationalInstruments_SourceModel_Modeling_Message.htm
[MessageConstructorRef]: http://xgen.amer.corp.natinst.com/DiagramSDK/html/M_NationalInstruments_SourceModel_Modeling_Message__ctor.htm
[SourceFileRef]: http://xgen.amer.corp.natinst.com/DiagramSDK/html/T_NationalInstruments_SourceModel_Modeling_SourceFile.htm

[MessagingRelationships]: MessagingRelationships.png

