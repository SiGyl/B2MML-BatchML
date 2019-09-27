# B2MML - Work Schedule

The Business To Manufacturing Markup Language (B2MML) is used courtesy of MESA International.

## Diagram Convention

[see here](../diagram-convention)

## Schema Scope

This document defines the information about work schedules and job lists.

This information is based on the data models and attributes defined in the ANSI/ISA 95.00.04 Enterprise/Control System Integration standard.

Contact ISA (The Instrumentation, System, and Automation Society) for copies of the standard. Additional information on the standard is available at www.isa.org. 

### Key Information Assumptions

The data represented in these schemas is derived from the UML model below.  This model is defined in the ANSI/ISA 95.00.04 standard.

The assumption is that information would be exchanged by either a work schedule or by a job list. 

[exchanged-work-schedule-and-job-list-information-model.jpgl](models/exchanged-work-schedule-and-job-list-information-model.jpg)

This schema uses a common schema for definition of elements that are used in multiple schemas, such as ID, Description, and Value.

This schema also includes the common schema definition for the requested segment response structure.

See the document defining the Common schema for definition of the common elements.   

### Type Definitions

The XML schema uses a model that defines simple and complex data types for each element.

The data types all follow the convention of a suffix of “Type” added to the element name.

Elements that have the same name in other B2MML schemas are also prefixed with “Op” to uniquely identify the extension group.  
  
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

### WorkSchedule

A work schedule is made up of a set of one or more work requests.

The work schedule also contains the information that defines the context of the schedule, such as start time, end time, location, and published date.

A work schedule may be made up of optional sub-work schedules. 

### WorkRequest

A work request defines set of job orders.

A work request may be made up of optional sub-work requests.

### JobList

A job list defines a set of job orders for a specific period of time and for specific resources.  

### JobOrder

A job order defines a job to be performed.

It defines the parameters, personnel, equipment, physical assets, and material requirements associated with the job order.  

It optionally defines the associated work master that defines the work to be performed for the job.  

### EquipmentRequirement

The job order may include one or more requirements for, or constraints upon, the equipment that the facility should use in the job.

Requirements can be as generic as materials of construction, or it can as specific as a particular piece of equipment.

Each of these requirements is defined in an EquipmentRequirement element and property.

### PersonnelRequirement

A personnel requirement and the associated personnel requirement property elements define to the number, type, duration, and scheduling of specific certifications and job classifications needed to support a job order.

### PhysicalAssetRequirement

The job order may include one or more requirements for, or constraints upon, the physical assets that the facility shall use in the job. 

### MaterialRequirement

A **MaterialRequirement** defines a requirement for a material to be produced or used.

A material requirement may include the total quantity of the material to be produced or consumed and unit of measure, such as 5000 Lbs, and an acceptable range for the quantity of material.

Material may be defined by Material Class ID, Material Definition ID, Material Lot ID, and/or Material Sublot ID.

A MaterialRequirement element includes an element that specifies if the material is to be consumed, produced, or is a consumable material 

### Resource Identification

[see here](../resource-identification)

### Element Definitions

#### WorkSchedule

*WorkScheduleType*

Contains a definition of a work schedule.

Including

- the hierarchy scope of the scheduled elements
- the publication date of the schedule
- the time range of the schedule
- the list of work requests that make up the schedule
- the optional sub work schedules.  

[work-schedule-element.jpg](elements/work-schedule-element.jpg)

#### WorkRequest

*WorkRequestType*

Contains a definition of a work request element of a work schedule.

Including

- the time range of the request
- the priority of the request
- the job orders of the request
- optional sub work requests

[work request element](elements/work-request-element.jpg)

#### JobList

*JobListType*

Contains a list of job orders for a specific resource (HierarchyScope) for a specific time period (StartTime and EndTime). 

[job-list-element.jpg](elements/job-list-element.jpg)

#### JobOrder

*JobOrderType*

Contains a definition of a job order.

Including

- an identification and version of the associated work master
- the time range of the request
- the expected duration of the request
- parameters for the job order
- the definition of the personnel, equipment, physical assets, material produced, material consumed, and consumables to be used in the job order. 

A *JobOrderType* may reference one or more **OperationsRequests**, or parts of an **OperationsRequest**.

- If it references the entire **OperationsRequest**, the **OperationsRequestID** contains the **OperationsRequest** ID.
- It it references part of an **OperationsRequest**, then the **SegmentRequirementID**, and the **SegmentRequirementIDs** are not unique across all **OperationsRequests**, then the **SegmentRequirementID** should contain the entire ID path to the **SegmentRequirementID**.
  - For Example: “SCH123/020/010” for the:
    - Segment Requirement 010, 
    - within Segment Requirement 020 
    - within Operations Request SCH123. 

[job-order-element.jpg](elements/job-order-element.jpg)

#### MaterialRequirement

*OpMaterialRequirementType*

Contains a definition of a material.

Including:

- an identification of the use of the material
-- the quantity of the material or a definition of required subsets identified by resource properties. 

A **MaterialRequirement** element may have a set of contained **AsemblyRequirement** elements to support hierarchical manufacturing bills. 

[op material requirement type](types/op-material-requirement-type.jpg)

#### MaterialRequirementProperty

*OpMaterialRequirementPropertyType*

Contains a definition of a subset of a material used in a segment requirement.

Including

- the value used to identify the subset
- the quantity of the material used

#### EquipmentRequirement

*OpEquipmentRequirementType*

Contains a definition of an equipment requirement for a segment requirement.

Including

- an identification of the quantity of the resource used
- or a definition of required subsets identified by resource properties

[op equipment requirement type](types/op-equipment-requirement-type.jpg)

#### EquipmentRequirementProperty

*OpEquipmentRequirementPropertyType*

Contains a definition of a subset of an equipment resource used in a segment requirement.

Including

- the value used to identify the subset
- the quantity of the resource used

#### PersonnelRequirement

*OpPersonnelRequirementType*

Contains a definition of a personnel requirement for a segment requirement.

Including

- an identification of the quantity of the resource used
- or a definition of required subsets identified by resource properties

[op personnel requirement type](types/op-personnel-requirement-type.jpg)

#### PersonnelRequirementProperty

*OpPersonnelRequirementPropertyType*

Contains a definition of a subset of a personnel resource used in a segment requirement.

Icluding

- the value used to identify the subset
- the quantity of the resource used

#### PhysicalAssetRequirement

*OpPhysicalAssetRequirementType*

Contains a definition of a physical asset requirement for a segment requirement.

Including

- an identification of the quantity of the resource used
- or a definition of required subsets identified by resource properties

[op physical asset requirement type](types/op-physical-asset-requirement-type.jpg)

#### PhysicalAssetRequirementProperty

*OpPhysicalAssetRequirementPropertyType*

Contains a definition of a subset of a physical asset resource used in a segment requirement.

Including

- the value used to identify the subset
- the quantity of the resource used

#### JobOrderParameter

*ParameterType*

Contains a definition of a job order parameter, as a ParameterType.

Including

- the value for the parameter

[parametertype](types/parameter-type.jpg)
 

## Transaction Elements

The following elements are defined to support the ISA 95 Part 5 transactions, using the transaction data types defined in the B2MML-Common.xsd schema. 

Work Schedule Elements      | Description
----------------------------|------------
**GetWorkSchedule**         | Get **WorkSchedule** definition. 
**ShowWorkSchedule**        | Returned information from the **GetWorkSchedule** message.
**ProcessWorkSchedule**     | Process **WorkSchedule** definition.
**AcknowledgeWorkSchedule** | Returned status from the **ProcessWorkSchedule** message.
**ChangeWorkSchedule**      | Change **WorkSchedule** definition.
**RespondWorkSchedule**     | Returned status from the **ChangeWorkSchedule** message.
**CancelWorkSchedule**      | Cancel **WorkSchedule** definition.
**SyncWorkSchedule**        | Published **WorkSchedule** definition.

Job List Elements           | Description
----------------------------|------------
**GetJobList**              | Get **JobList** definition. 
**ShowJobList**             | Returned information from the **GetJobList** message.
**ProcessJobList**          | Process **JobList** definition.
**AcknowledgeJobList**      | Returned status from the **ProcessJobList** message.
**ChangeJobList**           | Change **JobList** definition.
**RespondJobList**          | Returned status from the **ChangeJobList** message.
**CancelJobList**           | Cancel **JobList** definition.
**SyncJobList**             | Published **JobList** definition.
