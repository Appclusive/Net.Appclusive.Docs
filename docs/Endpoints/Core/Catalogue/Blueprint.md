A `Blueprint` wraps one or more `Models` so they can be made available in a `Catalogue` and be ordered and provisioned. There are two types of `Blueprints`:

* Basic
* Composite (? Advanced ?)

# Simple

A *simple* `Blueprint` wraps and describes only a single `Model`. This means that all defined attributes of that `Model` effectively become the input arguments in form of `ModelAttribute`s for that `Blueprint`. The simple blueprint comes as part of the Appclusive installation. Whenever a product designer creates a `Model` that can be provisioned as a single entity, the designer can associate the `Blueprint` with the simple blueprint and Appclusive takes care of the complete provisioning process (including approval).

![](media/SimpleBlueprint.png)

# Composite

A *composite* `Blueprint` wraps an arbitrary number of `Models` and/or other `Blueprints` into a single `Blueprint`. Therefore a *Composite* `Blueprint` can have a combination of input parameters that may be mapped to any `Model` attributes in any form that are required by the `Models`.

In addition the `Blueprint` designer may modify and map `Blueprint` input parameters in any way that he seems fit for that purpose. It is also possible to let a `Blueprint` define an input parameter that is used as a basis for other `Model` attributes, eg:

```
define input parameter 'com.example.Blueprint1.Size' := { S, M, L }
```

 ... can be mapped by the `Blueprint` to:

```
switch 'com.example.Appclusive.Product1.Size'
{
  case 'S':
    assign attribute 'com.example.VirtualMachine.CpuCount' = 1
    assign attribute 'com.example.VirtualMachine.MemoryGb' = 2
  case 'M':
    assign attribute 'com.example.VirtualMachine.CpuCount' = 2
    assign attribute 'com.example.VirtualMachine.MemoryG'b = 4
  case 'L':
    assign attribute 'com.example.VirtualMachine.CpuCount' = 4
    assign attribute 'com.example.VirtualMachine.MemoryGb' = 16
}
```

... where the attribute `com.example.VirtualMachine.CpuCount` and `com.example.VirtualMachine.MemoryGb` are specific attributes of a `Model` called `com.example.VirtualMachine` which the `Blueprint` whishes to make available for order.

The following image shows an example of a composite blueprint where several *shapes* are provisioned as part of a larger `Blueprint`:

![](media/CompositeBlueprint.png)

This `Blueprint` would be provided by the designer of the respectives `Model`s. The designer of the `Blueprint` is free to add an initial `Approval` step and handle the `Approval` once for all related entities. Each invocation of a simple blueprint will also trigger an `Approval` step in case the respective approval attributes are still present in the configuration.
