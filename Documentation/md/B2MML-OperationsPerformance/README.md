# B2MML - Operations Performance

The Business To Manufacturing Markup Language (B2MML) is used courtesy of MESA International.

## Diagram Convention

The schema diagrams using the following convention to illustrate the structure of the schema elements, the type of the elements and attributes, and the rules for optional elements and repetition. 

![diagram-convention](../diagram-convention.jpg)

## Schema Scope

This document defines the information about the definition of operations information that may be exchanged between business systems and manufacturing operations systems.

This information is based on the data models and attributes defined in the ANSI/ISA 95.00.02 Enterprise/Control System Integration standard.

Contact ISA (The Instrumentation, System, and Automation Society) for copies of the standard. Additional information on the standard is available at www.isa.org.  

### Key Information Assumptions

The data represented in these schemas is derived from the UML model below.  This model is defined in the ANSI/ISA 95.00.02-2010 standard.  The information model in the figure below is hierarchical, and the assumption is that any operations response information will always be within a contained Operations Performance object.  

[exchanged operations performance information model](models/exchanged-operations-performance-information-model.jpg)

This schema uses a common schema for definition of elements that are used in multiple schemas, such as ID, Description, and Value.  See the document defining the Common schema for definition of the common elements.

### Type Definitions

The XML schema uses a model that defines simple and complex data types for each element.  The data types all follow the convention of a suffix of “Type” added to the element name.  Elements that have the same name in other B2MML schemas are also prefixed with “Op” to uniquely identify the extension group. 
  
Schema definition:

````xsd
  
<xsd:element name = "OpPersonnelActual"  type = " OpPersonnelActualType"/>

<xsd:complexType name = "OpPersonnelActualType">
    <xsd:sequence>
      <xsd:element name = "PersonnelClassID" type = "PersonnelClassIDType" 
                                             minOccurs = "0" />
     …
</xsd:complexType>
````

The method is a modification of the “Venetian Blind Model”, defined in the book Professional XML Schemas, 2001, published by WROX (ISBN 1-861005-47-4).  It makes all of the type names global and usable in user derived works, without a loss of context or additional information required to identify the element as of being of the same type as related B2MML elements

### OperationsPerformance

An Operations Performance report is made up of a set of 1 or more operation responses.

The Operations Performance also contains the information that defines the context of the report, such as start time, end time, location, and published date. 

### OperationsResponse

Operation responses are the response from operations that is associated with an Operations Request.

There may be one or more operation responses for a single operation request if the facility needs to split the request into smaller elements of work.

For example a single request for the operations of “200 gears” may be reported on by 10 response objects of “20 gears" each because of manufacturing restrictions.

A result may include the status of the request, such as the percentage complete, a finished status, or an aborted status.

### SegmentResponse

The operations response for a specific segment of operations is defined as a segment response.

A segment response may be made up of zero or more sets of information on operations data, personnel actual, equipment actual, materials consumed actual, materials produced actual, and consumables actual.

A segment response may include an identification of the associated process segment, the actual starting and stopping time of the segment, and the duration of the segment. 

A **SegmentResponse** is also included as an optional element in an **OperationsRequest**.

In those cases the **SegmentResponse** defines elements that are to be returned with an **OperationsResponse**.

In this use it basically defines a template of information to be filled in and returned.

A segment response contains an element (**RequiredByRequestedSegmentResponse**) that is used in an **OperationsSchedule** to indicate if the including element is required or optional in a response from a request.

The value of the **RequiredByRequestedSegmentResponse** element may be extended on an application specific basis.

*NOTE: The **SegmentResponse** element (OpSegmentResponseType) is defined in the file* [B2MML-OperationsPerformanceTypes.xsd](../../../Schema/B2MML-OperationsPerformanceTypes.xsd)

### PersonnelActual

A PersonnelActual in an operations response identifies a personnel resource by class ID or by instance ID used during the specified segment of operations.  

### EquipmentActual

An EquipmentActual in an operations response identifies an equipment resource by class ID or instance ID used during the specified segment of operations.  

### PhysicalAssetActual

A PhysicalAssetActual in an operations response identifies a physical asset resource by class ID or instance ID used during the specified segment of operations.  

### MaterialActual

Material produced, material consumed, and consumable material actually used is identified in a MaterialActual.

This identifies a material resource by class ID, definition ID, Lot ID, and/or Sublot ID produced or consumed during the specified segment of operations.  

### Resource Identification

[see here](../resource-identification)

### Use within An operations schedule

The **SegmentResponseType** is also used in an operations schedule to define the requested segment response for a segment of operations.

This defines the structure and elements to be returned as a response for the operations schedule.

The **RequestedBySegmentResponse** attribute is used to indicate if the element is a required or optional element in a response. 

### Element Definitions

#### OperationsPerformance

*OperationsPerformanceType*

The top level element.

Contains a definition of a report on Operations Performance, including

- the hierarchy scope of the information
- the operations type
- the publication data of the performance report
- the ID of the associated operations schedule
- the duration of the Operations Performance
- and the list of operations responses making up the Operations Performance report

May include application specific defined elements.

[operations performance element](elements/operations-performance-element.jpg)

#### OperationsResponse

*OperationsResponseType*

Contains a definition of an operation response report, including

- the identification of an associated operations request
- the product produced
- the operations type
- the duration of the report
- and the segments making up the operations response

May include application specific defined elements.

May be a top level element for defined scopes. 

An **OperationsResponse** may reference a single **OperationsDefinition**, or part of an **OperationsDefinitions**.

- If it references the entire **OperationsDefinition**, the **OperationsDefinitionID** contains the **OperationsDefinition** ID.
- If it references part of an **OperationsDefinition** and the **OperationsSegment** IDs are not unique across all **OperationsDefinitions**, then the OperationsSegmentID should contain the entire ID path to the OperationsSegment.  

For Example: “R123/020/010” for the Operations Segment 010, within Operations Segment 020 within Operations Request R123.

[operations response element](elements/operations-response-element.jpg)

#### SegmentResponse

*OpSegmentResponseType*

Contains a definition of a report on a segment.

Includes

- duration
- operations segment type
- operations data
- personnel
- equipment
- physical assets
- material

*Note: The **RequiredByRequestedSegmentResponse** element is only used when this is included as part of an operations schedule schema.*

An **SegmentResponse** may reference a single **OperationsDefinition**, or part of an **OperationsDefinitions**.

- If it references the entire **OperationsDefinition**, the **OperationsDefinitionID** contains the **OperationsDefinition** ID.
- If it references part of an **OperationsDefinition** and the **OperationsSegment** IDs are not unique across all **OperationsDefinitions**, then the **OperationsSegmentID** should contain the entire ID path to the **OperationsSegment**.

For Example: “R123/020/010” for the Operations Segment 010, within Operations Segment 020 within Operations Request R123.

[segment response element](elements/segment-response-element.jpg)

#### EquipmentActual

*OpEquipmentActualType*

Contains a report on actual equipment resources used and use of the equipment.

May define the quantity of the resource used, or may contain a list of property definitions and quantities for each property subset. 

*Note: The **RequiredByRequestedSegmentResponse** element is only used when this is part of an operations schedule schema.*

[op equipment actual type](types/op-equipment-actual-type.jpg)
 
#### EquipmentActualProperty

*OpEquipmentActualPropertyType*

Contains a definition of actual equipment resources used, for a subset of the resource identified by a property value. 

Includes the quantity of the resources used. 

*Note: The **RequiredByRequestedSegmentResponse** element is only used when this is part of an operations schedule schema.*

[op equipment actual property type](types/op-equipment-actual-property-type.jpg)

#### MaterialActual

*OpMaterialActualType*

Contains a report on actual material resources used and use of the material.

May define the quantity of the material, or may contain a list of property definitions and quantities for each property subset. 

A **MaterialActual** element may have a set of contained **AssemblyActual** elements to support hierarchical manufacturing bills.

Note: The **RequiredByRequestedSegmentResponse** element is only used when this is part of an operations schedule schema.

[op material actual type](types/op-material-actual-type.jpg)

#### MaterialActualProperty

*OpMaterialActualPropertyType*

Contains a definition of actual material resources used, for a subset of the resource identified by a property value. 

Includes the quantity of the resource used. 

*Note: The **RequiredByRequestedSegmentResponse** element is only used when this is part of an operations schedule schema.*

[op material actual property type](types/op-material-actual-property-type.jpg)

#### PersonnelActual

*OpPersonnelActualType*

Contains a report on actual personnel resources used and use.

May define the quantity of the resource used, or may contain a list of property definitions and quantities for each property subset. 

Note: The **RequiredByRequestedSegmentResponse** element is only used when this is part of an operations schedule schema.

[op personnel actual type](types/op-personnel-actual-type.jpg)

#### PersonnelActualProperty

*OpPersonnelActualPropertyType*

Contains a definition of actual personnel resources used, for a subset of the resource identified by a property value. 

Includes the quantity of the resources used. 

*Note: The **RequiredByRequestedSegmentResponse** element is only used when this is part of an operations schedule schema.*

[op personnel actual property type](types/op-personnel-actual-property-type.jpg)

#### PhysicalAssetActual

*OpPhysicalAssetActualType*

Contains a report on actual physical asset resources used and use.

May define the quantity of the resource used, or may contain a list of property definitions and quantities for each property subset. 

*Note: The **RequiredByRequestedSegmentResponse** element is only used when this is part of an operations schedule schema.

[op physical asset actual type](types/op-physical-asset-actual-type.jpg)

#### PhysicalAssetActualProperty

*OpPhysicalAssetActualPropertyType*

Contains a definition of actual physical asset resources used, for a subset of the resource identified by a property value. 

Includes the quantity of the resources used. 

*Note: The **RequiredByRequestedSegmentResponse** element is only used when this is part of an operations schedule schema.

[op physical asset actual property type](types/op-physical-asset-actual-property-type.jpg)

#### SegmentData

*OpSegmentDataType*

Contains a definition of an operations data element, Includes the ID of the information and the value for the date, and nested segment data elements.

Note: The **RequiredByRequestedSegmentResponse** element is only used when this is part of an operations schedule schema.

[op segment data type](types/op-segment-data-type.jpg)
 
## Transaction Elements

The following elements are defined to support the ISA 95 Part 5 transactions, using the transaction data types defined in the B2MML-Common.xsd schema. 

Operations Performance Elements       | Description
--------------------------------------|------------
**GetOperationsPerformance**          | Get **OperationsPerformance** definition. 
**ShowOperationsPerformance**         | Returned information from the **GetOperationsPerformance** message.
**ProcessOperationsPerformance**      | Process **OperationsPerformance** definition.
**AcknowledgeOperationsPerformance**  | Returned status from the **ProcessOperationsPerformance** message.
**ChangeOperationsPerformance**       | Change **OperationsPerformance** definition.
**RespondOperationsPerformance**      | Returned status from the **ChangeOperationsPerformance** message.
**CancelOperationsPerformance**       | Cancel **OperationsPerformance** definition.
**SyncOperationsPerformance**         | Published **OperationsPerformance** definition.
