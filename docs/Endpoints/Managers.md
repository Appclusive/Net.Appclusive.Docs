# Appclusive as a resusable Component

Though Appclusive is available as ODATA REST based API it not necessarily has to be used via HTTP. In fact, the whole Appclusive framework is abstracted in a module called the *Core* and exposes its functionality via a set of so called *Managers*. In the default ODATA scenario each *Manager* is served by a corresponding *Controller*.

As an example the functionality for the `KeyNameValue` entities is contained in a manager named `KeyNameValueManager`. Its functionality is exposed by the `KeyNameValuesController`. By default every controller advertises the following *actions*:

* Get single entity
* Get entity set
* Create entity
* Update entity
* Update entity partially
* Delete entity
* Return default entity

Every manager can expose additional actions by defining nested classes that either derive from `PublicEntityManagerEntityActionBase` for operating on a single entity or from `PublicEntityManagerEntitySetActionBase` for operating on the complete entity set. Every ODATA Controller will automatically map these actions to ODATA actions (including the geneation of the complete model builder).

For other environments, such as the *CLI* (the Appclusive Command Line Interface sample implementation), the respective application will have to scan the available managers and expose their functionality accordingly.

Each manager must implement the `IPublicEntityManager` interface to be recognised as such. There are two default base implementations available depending on the aim of the manager:

1. `BasePublicEntityDataManager`. This manager is used for entities that should be persisted to the data layer. E.g. `KeyNameValueManager`.

2. `BasePublicEntityMemoryManager`. This manager is used for entities that only live in memory or aggregate information from several persisted entity sets. E.g. `HealthCheckManager`. 