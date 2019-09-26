# B2MML - Operations Schedule

The Business To Manufacturing Markup Language (B2MML) is used courtesy of MESA International.

## Diagram Convention

The schema diagrams using the following convention to illustrate the structure of the schema elements, the type of the elements and attributes, and the rules for optional elements and repetition. 

![diagram-convention](../diagram-convention.jpg)

## Schema Scope

This document defines the information about the definition of operations information that may be exchanged between business systems and manufacturing operations systems.

This information is based on the data models and attributes defined in the ANSI/ISA 95.00.02 Enterprise/Control System Integration standard.

Contact ISA (The Instrumentation, System, and Automation Society) for copies of the standard. Additional information on the standard is available at www.isa.org.  

### Key Information Assumptions

The data represented in these schemas is derived from the UML model below.  This model is defined in the ANSI/ISA 95 standard.  The information model in the figure below is hierarchical, and the assumption is that any Operations request information will always be within a contained Operations schedule object.  

[exchanged operations schedule information model](models/exchanged-operations-schedule-information-model.jpg)

This schema uses a common schema for definition of elements that are used in multiple schemas, such as ID, Description, and Value.  This schema also includes the Operations Performance schema definition for the requested segment response structure. See the document defining the Common schema for definition of the common elements.  See the document defining the Operations Performance schema for the definition of the requested segment response. 

### Type Definitions

The XML schema uses a model that defines simple and complex data types for each element.  The data types all follow the convention of a suffix of “Type” added to the element name.  Elements that have the same name in other B2MML schemas are also prefixed with “Op” to uniquely identify the extension group. 
  
Schema definition:

````xsd
  
<xsd:element name = "OpPersonnelRequirement"  type = " OpPersonnelRequirementType"/>

<xsd:complexType name = " OpPersonnelRequirementType">
    <xsd:sequence>
      <xsd:element name = "PersonnelClassID" type = "PersonnelClassIDType" 
                                             minOccurs = "0" />
     …
</xsd:complexType>
````

The method is a modification of the “Venetian Blind Model”, defined in the book Professional XML Schemas, 2001, published by WROX (ISBN 1-861005-47-4).  It makes all of the type names global and usable in user derived works, without a loss of context or additional information required to identify the element as of being of the same type as related B2MML elements.

### OperationsSchedule

An operations schedule is made up of a set of 1 or more Operations requests.

The Operations schedule also contains the information that defines the context of the schedule, such as start time, end time, location, and published date. The main structuring element of the schema definition is **OperationsSchedule.** 

### OperationsRequest

An operations request defines a request for Operations.  

An operations request identifies the associated Work Definition. An operations request must contain at least one segment requirement, even it spans all required operations.

### SegmentRequirement

An operations request is made up of one or more segment requirements.  Each segment requirement may correspond to, or reference, an identified process segment.  The segment requirement references the segment capability to which the associated personnel, equipment, physical assets materials, and segment parameters.

### SegmentResponse

An operations request may include a **SegmentResponse** element that defines the data to be returned after the execution of the segment.  

NOTE: The **SegmentResponse element** (*OpSegmentResponseType*) is defined in the file [B2MML-OperationsPerformanceTypes.xsd](../../../Schema/B2MML-OperationsPerformanceTypes.xsd)

### PersonnelRequirement

A personnel requirement and the associated personnel requirement property elements define to the number, type, duration, and scheduling of specific certifications and job classifications needed to support the current Operations request.  

### EquipmentRequirement

The Operations request may include one or more requirements for, or constraints upon, the equipment that the facility shall use in the Operations process for the scheduled item.

Requirements can be as generic as materials of construction, or it can as specific as a particular piece of equipment.  Each of these requirements is defined in an **EquipmentRequirement** element and property.

### PhysicalAssetRequirement

The Operations request may include one or more requirements for, or constraints upon, the physical assets that the facility shall use in the Operations process for the scheduled item.

### MaterialRequirement

A **MaterialRequirement** defines a requirement for a material to be produced or used.

A material requirement may include the total quantity of the material to be produced or consumed and unit of measure, such as 5000 Lbs, and an acceptable range for the quantity of material.

Material may be defined by

- Material Class ID
- Material Definition ID
- Material Lot ID and/or Material Sublot ID

A **MaterialRequirement** element includes an element that specifies if the material is to be consumed, produced, or is a consumable material 

### Resource Identification

The schemas follow the ANSI/ISA-95 standard by defining resources by class ID or instance ID, or by defining them by class ID and a property value that is used to define a subset of the resource.

For example, the figure below illustrates that a segment may require a certain number of milling machine, an equipment class.

Other segments may require a subset of milling machine, such as “Fine” milling machines only.

In the first case the class name, “Mill”, is sufficient to identify the resource required.

In the second case the class name, “Mill”, and property name and value, “Spec” and “Fine”, define the required resource.

Alternately a specific resource may be specified for an Operations schedule, such as requiring milling machine with ID=”Miller#1”.

[resource identification model](models/resource-identification-model.jpg)

### Element Definitions

#### OperationsSchedule

*OperationsScheduleType*

Contains a definition of an Operations schedule, including the hierarchy scope of the scheduled elements, the publication date of the schedule, the time range of the schedule, and the list of Operations requests that make up the schedule.  

[operations schedule element](elements/operations-schedule-element.jpg)

#### OperationsRequest

*OperationsRequestType*

Contains a definition of an Operations Request element of an Operations Schedule, including

- the associated operation to be performed
- the time range of the request
- the priority of the request
- the segment requirements of the request
- the definition of the expected segment response

An **OperationsRequest** may reference a single **OperationsDefinition**, or part of an **OperationsDefinitions**.

- If the **OperationsRequest** references the entire **OperationsDefinition**, the **OperationsDefinitionID** contains the **OperationsDefinition** ID.
- It the **OperationsRequest** references part of an **OperationsDefinition** and the **OperationsSegment** IDs are not unique across all **OperationsDefinitions**, then the **OperationsSegmentID** should contain the entire ID path to the **OperationsSegment**.

For Example: “R123/020/010” for the Operations Segment 010, within Operations Segment 020 within Operations Request R123.

[operations request element](elements/operations-request-element.jpg)

#### SegmentRequirement

*OpSegmentRequirementType*

Contains a definition of the schedule for a specific segment of Operations, including

- an identification of the associated process segment
- the time range of the request
- the expected duration of the request
- Operations parameters for the segment
- the definition of the following to be used in Operations
  - personnel
  - equipment
  - physical assets
  - material produced
  - material consumed
  - consumables

A **SegmentRequirement** may reference a single **OperationsDefinition**, or part of an **OperationsDefinitions**.

- If the **OperationsRequest** references the entire **OperationsDefinition**, the **OperationsDefinitionID** contains the **OperationsDefinition** ID.
- It the **OperationsRequest** references part of an **OperationsDefinition** and the **OperationsSegment** IDs are not unique across all **OperationsDefinitions**, then the **OperationsSegmentID** should contain the entire ID path to the **OperationsSegment**.
  - For Example: “R123/020/010” for the Operations Segment 010, within Operations Segment 020 within Operations Request R123.  

[segment requirement element](elements/segment-requirement-element.jpg)

#### MaterialRequirement

*OpMaterialRequirementType*

Contains a definition of a material, including

- an identification of the use of the material
- the quantity of the material or a definition of required subsets identified by resource properties. 

A **MaterialRequirement** element may have a set of contained AsemblyRequirement elements to support hierarchical manufacturing bills. 

[material requirement element](elements/material-requirement-element.jpg)

#### MaterialRequirementProperty

*OpMaterialRequirementPropertyType*

Contains a definition of a subset of a material used in a segment requirement, including

- the value used to identify the subset
- the quantity of the material used

#### EquipmentRequirement

*OpEquipmentRequirementType*

Contains a definition of an equipment requirement for a segment requirement, including

- an identification of the quantity of the resource used
- or a definition of required subsets identified by resource properties

[equipment requirement element](elements/equipment-requirement-element.jpg)

#### EquipmentRequirementProperty

*OpEquipmentRequirementPropertyType*

Contains a definition of a subset of an equipment resource used in a segment requirement, including

- the value used to identify the subset 
- the quantity of the resource used.

#### PersonnelRequirement

*OpPersonnelRequirementType*

Contains a definition of a personnel requirement for a segment requirement, including

- an identification of the quantity of the resource used
- or a definition of required subsets identified by resource properties

[personnel requirement element](elements/personnel-requirement-element.jpg)

#### PersonnelRequirementProperty

*OpPersonnelRequirementPropertyType*

Contains a definition of a subset of a personnel resource used in a segment requirement, including

- the value used to identify the subset
- the quantity of the resource used

#### PhysicalAssetRequirement

*OpPhysicalAssetRequirementType*

Contains a definition of a physical asset requirement for a segment requirement, including

- an identification of the quantity of the resource used
- or a definition of required subsets identified by resource properties

[physical asset requirement element](elements/physical-asset-requirement-element.jpg)

#### PhysicalAssetRequirementProperty

*OpPhysicalAssetRequirementPropertyType*

Contains a definition of a subset of a physical asset resource used in a segment requirement, including

- the value used to identify the subset
- the quantity of the resource used

#### SegmentParameter

*ParameterType*

Contains a definition of a segment parameter, as a *ParameterType*, including the value for the parameter. 
 
[segment parameter element](elements/segment-parameter-element.jpg)

### Transaction Elements

The following elements are defined to support the ISA 95 Part 5 transactions, using the transaction data types defined in the B2MML-Common.xsd schema. 

Operations Schedule Elements          | Description
--------------------------------------|------------
**GetOperationsSchedule**             | Get **OperationsSchedule** definition. 
**ShowOperationsSchedule**            | Returned information from the **GetOperationsSchedule** message.
**ProcessOperationsSchedule**         | Process **OperationsSchedule** definition.
**AcknowledgeOperationsSchedule**     | Returned status from the **ProcessOperationsSchedule** message.
**ChangeOperationsSchedule**          | Change **OperationsSchedule** definition.
**RespondOperationsSchedule**         | Returned status from the **ChangeOperationsSchedule** message.
**CancelOperationsSchedule**          | Cancel **OperationsSchedule** definition.
**SyncOperationsSchedule**            | Published **OperationsSchedule** definition.
