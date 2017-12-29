---
layout: page
title: "The Name Resolution System"
category: concepts
order: 9
---

The Diagram SDK name resolution system is responsible for linking items across scopes, allowing diagrams to use external resources, such as classes and methods defined in a separate assembly.

## Name Resolution System Architecture

The following diagram illustrates the high-level architecture of the name resolution system.

![NamingRelationships]

## Name Resolution System Theory of Operation

The name resolution system allows you to assign [QualifiedNames][QualifiedNameRef] to [Elements][ElementRef] so that they can be found by other Elements. The name resolution system comes into play when one Element needs to find another Element that is not part of its [Components][ComponentConcept] collection. The process of looking up a QualifiedName to find an Element, known as resolving, is the only mechanism the Diagram SDK supports for lazily referring to Elements.

### Example

Suppose you have a diagram, MyDiagram, that belongs to a SourceFile, MySourceFile. MyDiagram needs to call a diagram, ExternalDiagram, defined in another SourceFile, ExternalSourceFile. In this case, MyDiagram is an IQualifiedSource that depends on an IQualifiedObject, ExternalDiagram, by way of an IQualifiedDependency. As an IQualifiedSource, MyDiagram must use the name resolution system to locate an IQualifiedScope named `ExternalSourceFile`, then within that scope, it must locate an Element with the IQualifiedName `ExternalDiagram`, as shown in the following illustration.

![NamingProcess]

## Primary Types that Participate in the Name Resolution System

The Diagram SDK naming resolution system is woven throughout the [DiOM][DiOMConcept]. The majority of the functionality is implemented as interfaces to ensure flexibility and to allow objects outside of the DiOM to be resolved by name. The following table describes the core naming classes.


| Class                 |Description
|:--------------------- |:------------------------------------------------------------------- |
| QualifiedName         | A fully-qualified name for an IQualifiedTarget, including a path to the IQualifiedTarget through IQualifiedScopes, separated by " : " delimiters. |
| IQualifiedTarget	    | An object with a QualifiedName in an IQualifiedScope                |
| IQualifiedScope	    | Interface implemented by objects that contain IQualifiedTargets and resolve QualifiedNames into IQualifiedTargets |
| IQualifiedDependent   | An object that depends on an IQualifiedTarget by name               |
| Envoy                 | Represents a QualifiedTarget in a Project                           |
| Definition	        | An abstract class for specifying named objects                      |

### QualifiedName

[QualifiedName][QualifiedNameRef] is a sealed class that represents a name for an object or a path to a named object through a hierarchy of [IQualifiedScopes][IQualifiedScopeRef]. A QualifiedName consists of a sequence of one or more parts. The first part in the sequence is called the Head and the remaing parts are called the Tail. A part is a string that can contain any character. A QualifiedName used as a path can be relative or absolute. An absolute QualifiedName starts with a zero-length root part, otherwise the QualifiedName is relative.

During [parsing and generating][Tutorial_Parsing], a QualifiedName is converted to an XML string and back. The syntax for a QualifiedName written and read between an XML attribute is:

```C#
	QualifiedName := RootPart? Parts
	RootPart := Separator
	Parts := Part | QualifiedName Separator Part
	Part := ([a-zA-Z0-9_\\.])+
	Separator := ':' | '.'
```

The syntax of a QualifiedName when generating and parsing into a large block of text is different because there is no fixed delimiter. An example of such a syntax usage would be for parsing and generating the DataType names within an inline cluster DataType definition.

```C#
	QualifiedName := RootPart? Parts
	RootPart := Separator
	Parts := Part | QualifiedName Separator Part
	Part := ([a-zA-Z0-9_\\.])+
	Separator := ':' | '.'
```

The QualifiedName class includes [properties and methods][PropertiesAndMethodsRef] to inspect the parts of a name, compare, and match subsets of one qualified name with another.

### IQualifiedObject

[IQualifiedObject][IQualifiedObjectRef] is an interface implemented by objects that are named within a scope. Some examples of objects that implement IQualifiedObject are [Namespace][NamespaceRef] and [Definition][DefinitionRef]. The name of an IQualifiedObject is generally a leaf name, but this is not a requirement.








[Tutorial_Parsing]: http://xgen.amer.corp.natinst.com/DiagramSDK/Tutorial_EnableSave.html

[ComponentConcept]: ../InProgress.html
[DiOMConcept]: ../InProgress.html



[DefinitionRef]: http://xgen.amer.corp.natinst.com/DiagramSDK/html/T_NationalInstruments_SourceModel_Modeling_Definition.htm
[ElementRef]: http://xgen.amer.corp.natinst.com/DiagramSDK/html/T_NationalInstruments_SourceModel_Modeling_Element.htm
[IQualifiedObjectRef]: http://xgen.amer.corp.natinst.com/DiagramSDK/html/T_NationalInstruments_SourceModel_Naming_IQualifiedObject.htm
[IQualifiedScopeRef]: http://xgen.amer.corp.natinst.com/DiagramSDK/html/T_NationalInstruments_SourceModel_Naming_IQualifiedScope.htm
[NamespaceRef]: http://xgen.amer.corp.natinst.com/DiagramSDK/html/T_NationalInstruments_SourceModel_Modeling_Namespace.htm
[PropertiesAndMethodsRef]: http://xgen.amer.corp.natinst.com/DiagramSDK/html/AllMembers_T_NationalInstruments_SourceModel_Naming_QualifiedName.htm	
[QualifiedNameRef]: http://xgen.amer.corp.natinst.com/DiagramSDK/html/T_NationalInstruments_SourceModel_Naming_QualifiedName.htm





[NamingProcess]: NamingProcess.png
[NamingRelationships]: NamingRelationships.png