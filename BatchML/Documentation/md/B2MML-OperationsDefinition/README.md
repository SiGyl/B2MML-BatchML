# B2MML - Operations Definition

The Business To Manufacturing Markup Language (B2MML) is used courtesy of MESA International.

## Diagram Convention

[see here](../diagram-convention)

## Schema Scope

[B2MML-OperationsDefinition.xsd](../../../Schema/B2MML-OperationsDefinition.xsd)

This document defines the information about the definition of operations information that may be exchanged between business systems and manufacturing operations systems.

This information is based on the data models and attributes defined in the ANSI/ISA 95.00.02 Enterprise/Control System Integration standard.

Contact ISA (The Instrumentation, System, and Automation Society) for copies of the standard. Additional information on the standard is available at www.isa.org.  

### Key Information Assumptions

The data represented in these schemas is derived from the UML model below.  This model is defined in the ANSI/ISA 95.00.02 standard.  The information model in the figure below is hierarchical with references to, but does not include, the bill of materials and the bill of resources. The key assumption is that the information will be accessed by an Operations Definition.

[exchanged operations definition information model](models/exchanged-operations-definition-information-model.jpg)

This schema uses a common schema for definition of elements that are used in multiple schemas, such as ID, Description, and Value.  See the document defining the Common schema for definition of the common elements.

### Type Definitions

The XML schema uses a model that defines simple and complex data types for each element.  The data types all follow the convention of a suffix of “Type” added to the element name.  Elements that have the same name in other B2MML schemas are also prefixed with “Op” to uniquely identify the extension group. 
  
Schema definition:

````xsd
  
<xsd:element name = "OpPersonnelSpecification"  type = " OpPersonnelSpecificationType"/>

<xsd:complexType name = "OpPersonnelSpecificationType">
    <xsd:sequence>
      <xsd:element name = "PersonnelClassID" type = "PersonnelClassIDType" 
                                             MinOccurs = "0" />
     …
</xsd:complexType>
````

The method is a modification of the “Venetian Blind Model”, defined in the book Professional XML Schemas, 2001, published by WROX (ISBN 1-861005-47-4).  It makes all of the type names global and usable in user derived works, without a loss of context or additional information required to identify the element as of being of the same type as related B2MML elements

### OperationsDefinition

The main structuring element of the schema definition is OperationsDefinition.

OperationsDefinition is the container object for exchanged information and includes references to the Work Definitions, Bill Of Materials, and Bill Of Resources.

The term Work Definitions is used in ANSI/ISA-95.00.01 to indicate the information that used within operations, such as assembly instructions, flow sheets, or recipes.

Additional information exists in the bill of materials, bill of resources, and manufacturing operations systems, but is not defined in the exchange schemas.  

### ManufacturingBill 

A manufacturing bill identifies a material or material class that is needed for operations. 

The manufacturing bill includes all uses of the material in production of the product, while the operations segment’s material specification defines just the amount used. 

For example: a manufacturing bill may identify 55 Type C left threaded screws, where 20 are used in one operations segment, 20 in another operations segment, and 15 used in a third operations segment.

ManufacturingBill elements define materials that make up the manufacturing bill.  These materials may be identified by material class or by material definition. 

### OperationsSegment

The operations segment information defines what personnel, equipment, physical asset, or material resources are required for execution of the operations segment.

It does this by defining the classes of resources, or in some cases the exact instance of a resource required.

For example, an assembly segment may require 1 assembler for 2 hours, and 1 assembly machine for 2 hours.  In some industries the exact assembly machine may have to be specified, such as “AssemblyMachine#1”.

An operations segment also defines parameters that may be specified when the segment is executed, such as a color specification or manufacturing options. 

#### PersonnelSpecification

PersonnelSpecification elements define the personnel resources, by class or instance, required for production of the product within a operations segment.

Such as 2 hours of a painter for a paint segment for a lot size of one widget. 

#### EquipmentSpecification

EquipmentSpecification elements define the equipment resources, by class or instance, required for production of the product within an operations segment, such as 2 hours for a paint station for a lot size of one widget.

#### PhyscialAssetSpecification

PhysicalAssetSpecification elements define the physical assets resources, by class or instance, required for operations, such as 2 hours for a paint station for a lot size of one widget.

#### MaterialSpecification

MaterialSpecification elements define the material resources, by material class or material definition, required for production of the product within an operations segment, such as 30 Kg of cooking oil (material class) required for the cooking segment for a lot size of 50 Kg.  

### Resource Identification

[see here](../resource-identification)

## Element Definitions

### OperationsDefinitionInformation

*OperationsDefinitionInformationType*

Contains a list of operation definitions. Includes the hierarchy scope of the information, and the date of publication of the information.

[operations definition information element](elements/operations-definition-information-element.jpg)

### OperationsDefinition

*OperationsDefinitionType*

Contains an operations definition.

Includes the hierarchy scope of the information, the date of publication of the information, the list of materials in the manufacturing bill, the identification of the bill material, the identification of the bill of resources, and the definition of operations segments.

[operations definition element](elements/operations-definition-element.jpg)
 
### OperationsMaterialBill

*OperationsMaterialBillType*

Contains the list of operations material bill items. 

[operations material bill type](types/operations-material-bill-type.jpg)

### OperationsMaterialBillItem

*OperationsMaterialBillItemType*

Contains a definition of a material bill item, including the quantity of the material needed, an identification of the material class or definition, any assembly bill of material items, and the corresponding bill of material ID. 

An OperationsMaterialBillItem element may have a set of contained AssemblyBillOfMaterialItem elements to support hierarchical material bills.

[operations-material-bill-item-type.jpg](types/operations-material-bill-item-type.jpg)

### EquipmentSpecification

*OpEquipmentSpecificationType*

Contains a definition of the equipment resources required for the operations segment.

Includes the identification of the class or instance of the resources, the quantity of the resource, and the property specification if required to identify the resource. 

### EquipmentSpecificationProperty

*OpEquipmentSpecificationPropertyType*

Contains a definition of an equipment property required for the operations segment, including the quantity of the resource, and a value used to identify the subset of the class.

### MaterialSpecification

*OpMaterialSpecificationType*

Contains a definition of the material resources required for the operations segment.

Includes the identification of the class or instance of the resources, the quantity of the resource, the use (consumed, produced), any specification assemblies, and the property specification if required to identify the resource. 

A **ManufacturingSpecification** element may have a set of contained **AssemblySpecification** elements to support hierarchical manufacturing bills. 

### MaterialSpecificationProperty

*OpMaterialSpecificationPropertyType*

Contains a definition of a material property required for the operations segment, including the quantity of the resource, and a value used to identify the subset of the class.

### PersonnelSpecification

*OpPersonnelSpecificationType*

Contains a definition of the personnel resources required for the operations segment.

Includes the identification of the class or instance of the resources, the quantity of the resource, and the property specification if required to identify the resource. 

### PersonnelSpecificationProperty

*OpPersonnelSpecificationPropertyType*

Contains a definition of a personnel property required for the operations segment, including the quantity of the resource, and a value used to identify the subset of the class.

### PhysicalAssetSpecification

*OpPhysicalAssetSpecificationType*

Contains a definition of the physical asset resources required for the operations segment.

Includes the identification of the class or instance of the resources, the quantity of the resource, and the property specification if required to identify the resource. 

### PhysicalAssetSpecificationProperty

*OpPhysicalAssetSpecificationPropertyType*

Contains a definition of a physical asset property required for the operations segment, including the quantity of the resource, and a value used to identify the subset of the class.

### OperationsSegment

*OperationsSegmentType*

Contains a definition of an operations segment, including the quantity of resources required for the segment (per unit of production), an estimated duration of the segment, an identification of the corresponding process segment, parameters associated with the segment, the segment dependencies, and any encapsulated segments.

May also contain application specific elements.

[operations segment element](elements/operations-segment-element.jpg)

## Transaction Elements

The following elements are defined to support the ISA 95 Part 5 transactions, using the transaction data types defined in the B2MML-Common.xsd schema. 

Operations Definition Information Elements      | Description
------------------------------------------------|------------
**GetOperationsDefinitionInformation**          | Get **OperationsDefinitionInformation** definitions. 
**ShowOperationsDefinitionInformation**         | Returned information from the **GetOperationsDefinitionInformation** message.
**ProcessOperationsDefinitionInformation**      | Process **OperationsDefinitionInformation** definitions.
**AcknowledgeOperationsDefinitionInformation**  | Returned status from the **ProcessOperationsDefinitionInformation** message.
**ChangeOperationsDefinitionInformation**       | Change **OperationsDefinitionInformation** definitions.
**RespondOperationsDefinitionInformation**      | Returned status from the **ChangeOperationsDefinitionInformation** message.
**CancelOperationsDefinitionInformation**       | Cancel **OperationsDefinitionInformation** definitions.
**SyncOperationsDefinitionInformation**         | Published **OperationsDefinitionInformation** definitions.


Operations Definition Elements                  | Description
------------------------------------------------|------------
**GetOperationsDefinition**                     | Get an **OperationsDefinition definition. 
**ShowOperationsDefinition**                    | Returned information from the **GetOperationsDefinition** message.
**ProcessOperationsDefinition**                 | Process an **OperationsDefinition** definition.
**AcknowledgeOperationsDefinition**             | Returned status from the **ProcessOperationsDefinition** message.
**ChangeOperationsDefinition**                  | Change an **OperationsDefinition** definition.
**RespondOperationsDefinition**                 | Returned status from the **ChangeOperationsDefinition** message.
**CancelOperationsDefinition**                  | Cancel an **OperationsDefinition** definition.
**SyncOperationsDefinition**                    | Published **OperationsDefinition** definition.
