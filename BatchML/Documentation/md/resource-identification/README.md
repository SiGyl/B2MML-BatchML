# Resource Identification

The schemas follow the ANSI/ISA-95 standard by defining resources by class ID or instance ID, or by defining them by class ID and a property value that is used to define a subset of the resource.

For example, the figure below illustrates that a segment may require a certain number of milling machine, an equipment class.

Other segments may require a subset of milling machine, such as “Fine” milling machines only.

In the first case the class name, “Mill”, is sufficient to identify the resource required.

In the second case the class name, “Mill”, and property name and value, “Spec” and “Fine”, define the required resource.

[resource identification model](models/resource-identification-model.jpg)
