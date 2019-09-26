# B2MML - Work Performance

The Business To Manufacturing Markup Language (B2MML) is used courtesy of MESA International.

## Diagram Convention

The schema diagrams using the following convention to illustrate the structure of the schema elements, the type of the elements and attributes, and the rules for optional elements and repetition. 

![diagram-convention](../diagram-convention.jpg)

## Schema Scope

This document defines the information about Work Performance information.

This information is based on the data models and attributes defined in the ANSI/ISA 95.00.02 Enterprise/Control System Integration standard.

Contact ISA (The Instrumentation, System, and Automation Society) for copies of the standard. Additional information on the standard is available at www.isa.org.  

### Key Information Assumptions

The data represented in these schemas is derived from the UML model below.  This model is defined in the ANSI/ISA 95.00.04 standard.

The information model in the figure below is hierarchical, and the assumption is that the information may be accessed by Work Response or by Job Response.  

[exchanged work performance and job response information model](models/exchanged-work-performance-and-job-response-information-model.jpg)

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

The method is a modification of the “Venetian Blind Model”, defined in the book Professional XML Schemas, 2001, published by WROX (ISBN 1-861005-47-4).  It makes all of the type names global and usable in user derived works, without a loss of context or additional information required to identify the element as of being of the same type as related B2MML elements.

### WorkPerformance

A Work Performance report is made up of a set of one or more work responses.  The Work Performance also contains the information that defines the context of the report, such as start time, end time, location, and published date.

### WorkResponse

Work responses are collections of job responses.  A response may include the type of work, and the start time, end time.  

### JobResponse

A **JobResponse** is the response from operations about the execution of a job order.  

### EquipmentActual

An equipment actual in a Job Response identifies an equipment resource by class ID or instance ID used during execution of the job.  

### PersonnelActual

A personnel actual in a Job Response identifies a personnel resource by class ID or by instance ID used during execution of the job.  

### PhysicalAssetActual

A physical asset actual in a Job Response identifies a physical asset resource by class ID or instance ID used during execution of the job.  

### MaterialActual

A material produced, material consumed, or consumable material actually used is identified in a MaterialActual.

This identifies a material resource by class ID, definition ID, Lot ID, and/or Sublot ID produced or consumed during execution of the job.  

### Resource Identification

[see here](../resource-identification)

### Element Definitions

#### WorkPerformance

*WorkPerformanceType*

The top level element.

Contains a definition of a report on Work performance, including

- the hierarchy scope of the information
- the work type
- the publication data of the report
- the ID of the associated work schedule
- the duration of the work performance

May include application specific defined elements. 

[work performance element](elements/work-performance-element.jpg)

#### WorkResponse

*WorkResponseType*

Contains a definition of a Work Response report, including

- the identification of an associated work request
- the type of work (Production, Maintenance, Inventory, and Test)
- the duration of the report
- and the response state.  

[work response element](elements/work-response-element.jpg)

#### JobResponse

*JobResponseType*

Contains a definition of a report on the result of execution of a job order.

Includes

- the duration
- work type
- an ID and version of the associated Work Directive
- the start and end time of job execution, personnel, equipment, physical assets, and material used in the execution of the job order. 

A *JobResponseType* may reference one or more **OperationsRequests**, or parts of an **OperationsRequest**.

- If it references the entire **OperationsRequest**, the **OperationsRequestID** contains the **OperationsRequest** ID.
- If it references part of an **OperationsRequest**, then the **SegmentRequirementID**, and the **SegmentRequirementIDs** are not unique across all **OperationsRequests**, then the **SegmentRequirementID** should contain the entire ID path to the **SegmentRequirementID**.
  - For Example: “SCH123/020/010” for the:
    - Segment Requirement 010, 
    - within Segment Requirement 020 
    - within Operations Request SCH123.

[job response element](elements/job-response-element.jpg)

#### EquipmentActual

*OpEquipmentActualType*

Contains a report on actual equipment resources used and use of the equipment.

May define the quantity of the resource used, or may contain a list of property definitions and quantities for each property subset. 

*Note: The **RequiredByRequestedJobResponse** element is only used when this is part of an Operations Schedule schema.

[equipment actual element](elements/equipment-actual-element.jpg)

#### EquipmentActualProperty

*OpEquipmentActualPropertyType*

Contains a definition of actual equipment resources used, for a subset of the resource identified by a property value.

Includes the quantity of the resources used. 

#### MaterialActual

*OpMaterialActualType*

Contains a report on actual material resources used and use of the material.

May define the quantity of the material, or may contain a list of property definitions and quantities for each property subset. 

A **MaterialActual** element may have a set of contained AssemblyActual elements to support hierarchical manufacturing bills. 

*Note: The **RequiredByRequestedJobResponse** element is only used when this is part of an Operations Schedule schema.*

[material actual element](elements/material-actual-element.jpg)

#### MaterialActualProperty

*OpMaterialActualPropertyType*

Contains a definition of actual material resources used, for a subset of the resource identified by a property value.

Includes the quantity of the resource used. 

#### PersonnelActual

*OpPersonnelActualType*

Contains a report on actual personnel resources used and use.

May define the quantity of the resource used, or may contain a list of property definitions and quantities for each property subset. 

*Note: The **RequiredByRequestedJobResponse** element is only used when this is part of an Operations Schedule schema.*

[personnel actual element](elements/personnel-actual-element.jpg)

#### PersonnelActualProperty

*OpPersonnelActualPropertyType*

Contains a definition of actual personnel resources used, for a subset of the resource identified by a property value.

Includes the quantity of the resources used. 

#### PhysicalAssetActual

*OpPhysicalAssetActualType*

Contains a report on actual physical asset resources used and use.

May define the quantity of the resource used, or may contain a list of property definitions and quantities for each property subset. 

*Note: The **RequiredByRequestedJobResponse** element is only used when this is part of an Operations Schedule schema.*

[physical asset actual element](elements/physical-asset-actual-element.jpg)

#### PhysicalAssetActualProperty

*OpPhysicalAssetActualPropertyType*

Contains a definition of actual physical asset resources used, for a subset of the resource identified by a property value.

Includes the quantity of the resources used. 

#### JobResponseData

*OpSegmentDataType*

Contains a definition of a job response data element, includes

- the ID of the information
- the value for the date
- nested segment data elements.

*Note: The **RequiredByRequestedJobResponse** element is only used when this is part of an Operations Schedule schema.*

[job response data element](elements/job-response-data-element.jpg)

### Transaction Elements

The following elements are defined to support the ISA 95 Part 5 transactions, using the transaction data types defined in the B2MML-Common.xsd schema. 

Work Performance Elements         | Description
----------------------------------|------------
**GetWorkPerformance**            | Get **WorkPerformance** definition. 
**ShowWorkPerformance**           | Returned information from the **GetWorkPerformance** message.
**ProcessWorkPerformance**        | Process **WorkPerformance** definition.
**AcknowledgeWorkPerformance**    | Returned status from the **ProcessWorkPerformance** message.
**ChangeWorkPerformance**         | Change **WorkPerformance** definition.
**RespondWorkPerformance**        | Returned status from the **ChangeWorkPerformance** message.
**CancelWorkPerformance**         | Cancel **WorkPerformance** definition.
**SyncWorkPerformance**           | Published **WorkPerformance** definition.

Job Response Elements             | Description
----------------------------------|------------
**GetJobResponse**                | Get **JobResponse** definition. 
**ShowJobResponse**               | Returned information from the **GetJobResponse** message.
**ProcessJobResponse**            | Process **JobResponse** definition.
**AcknowledgeJobResponse**        | Returned status from the **ProcessJobResponse** message.
**ChangeJobResponse**             | Change **JobResponse** definition.
**RespondJobResponse**            | Returned status from the **ChangeJobResponse** message.
**CancelJobResponse**             | Cancel **JobResponse** definition.
**SyncJobResponse**               | Published **JobResponse** definition.
