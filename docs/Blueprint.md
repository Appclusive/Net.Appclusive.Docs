A `Blueprint` wraps one or more `Models` so they can be made available in a `Catalogue` and be ordered and provisioned. There are two types of `Blueprints`:

* Simple (? Basic ?)
* Composite (? Complex, Combined, Advanced ?)

# Simple

A *simple* `Blueprint` wraps and describes only a single `Model`. This means that all defined attributes of that `Model` effectively become the input parameters (? properties ?) for that `Blueprint`.

# Composite

A *composite* `Blueprint` wraps an arbitrary number of `Models` and/or other `Blueprints` into a single `Blueprint`. Therefore a *composite* `Blueprint` can have a combination of input parameters that may be mapped to any `Model` attributes in any form that are required by the `Models`.

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
