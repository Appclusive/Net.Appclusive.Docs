A `Catalogue` consists of zero or more `CatalogueItems` where each `CatalogueItem` references a given `Blueprint`. Multiple `CatalogueItems` can reference the same `Blueprint`. Each `CatalogueItem` can define any number of input parameters that override or preset input parameters of a  `Blueprint`. 

Taken the example from earlier, we put the `Blueprint` `com.example.Blueprint1` into a `CatalogueItem`. In its corresponding `CatalogueItem` we define the following *constraint* input parameter:

```
define input parameter 'com.example.Blueprint1.Size' := { M, L }
```

By specifying `M` and `L` as *arguments* for this input parameter we limit the choice the `blueprint` would normally offer. Furthermore it is possible to define *constraints* on a `Catalogue` as a whole so these input parameters apply to all `CatalogueItems` (and therefore all `blueprints`) of that `Catalogue`.