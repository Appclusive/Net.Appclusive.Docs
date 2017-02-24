# Development

## General

The following classes have to be implemented to expose functionality as OData service

* `_ENTITY_` (Persistence representation)
* `_ENTITY_` (Model class)
* `_ENTITY_ValidatorTest` (Test class for entity validator)
* `_ENTITY_Validator` (Entity validator)
* `_ENTITY_ManagerTest` (Test class for entity manager)
* `_ENTITY_Manager` (Entity manager)
* `_ENTITY_sController`  (OData controller)

`_ENTITY_` is a placeholder for the name of the corresponding entity.

## DataAccess

The class for data access resides in project `Net.Appclusive.Internal` under `DataAccess\_DOMAIN_\_ENTITY_.cs`, where `_DOMAIN_` is a placeholder for the corresponding domain.

The following steps have only to be executed, if the `_ENTITY_` will be persisted in the database.

* Register entity set in `ApcDbContext`
* Add new migration by executing `Add-Migration MigrationName` in `Package Manager Console` of Visual Studio 2015

**Example**

```
using System.ComponentModel.DataAnnotations;

namespace Net.Appclusive.Internal.DataAccess._DOMAIN_
{
    public class _ENTITY_ : NonTenantDataAccessBase
    {
        [Required]
        public string Value { get; set; }
    }
}
```

## Model

The DataAccess class resides in project `Net.Appclusive.Public` under `Domain\_DOMAIN_\_ENTITY_.cs`, where `_DOMAIN_` is a placeholder for the corresponding domain.
This class has to be implemented even if there is no representation of the `_ENTITY_` in the database. 

**Example**

```
using System.ComponentModel.DataAnnotations;

namespace Net.Appclusive.Public.Model._DOMAIN_
{
    public class _ENTITY_ : BaseEntity
    {
        [Required]
        public string Value { get; set; }
    }
}
``` 

## Validator

The Validator class resides in project `Net.Appclusive.Core` under `Domain\_DOMAIN_\_ENTITY_Validator.cs`, where `_DOMAIN_` is a placeholder for the corresponding domain.

**Example**

```
using System.Diagnostics.Contracts;
using Net.Appclusive.Public.Model._DOMAIN_;
using Net.Appclusive.Public.Validation;

namespace Net.Appclusive.Core.Domain._DOMAIN_
{
    public class _ENTITY_Validator : BaseEntityValidator<_ENTITY_>
    {
        public override KeyNameValue ForCreate(_ENTITY_ entityToBeCreated)
        {
            Contract.Requires(!string.IsNullOrWhiteSpace(entityToBeCreated.Value));

            return base.ForCreate(entityToBeCreated);
        }

        public override _ENTITY_ ForUpdate(_ENTITY_ modifiedEntity, _ENTITY_ originalEntity)
        {
            Contract.Requires(modifiedEntity.Id == originalEntity.Id);

            return base.ForUpdate(modifiedEntity, originalEntity);
        }
    }
}
```

## Manager

The Manager class resides in project `Net.Appclusive.Core` under `Domain\_DOMAIN_\_ENTITY_Manager.cs`, where `_DOMAIN_` is a placeholder for the corresponding domain.

**Example**

```
using System;
using Net.Appclusive.Core.DbContext;
using Net.Appclusive.Public.Constants;
using Net.Appclusive.Public.Model._DOMAIN_;
using Net.Appclusive.Core.Security;

namespace Net.Appclusive.Core.Domain._DOMAIN_
{
    public partial class _ENTITY_Manager : BaseDataAccessEntityManager<_ENTITY_, Internal.DataAccess._DOMAIN_._ENTITY_>
    {
        public _ENTITY_Manager(BaseDataAccessEntityManagerCtorParams ctorParams)
            : base(ctorParams)
        { 
            // there is nothing to do here
        }
		
		public override ActionConfigurationMap ActionMap { get; } = ActionConfigurationMap.For<JobManager>();

        public override _ENTITY_ Template()
        {
            var template = base.Template();

            template.Value = Model.ARBITRARY_STRING_VALUE;

            return template;
        }
    }
}
```

## ODataController

The ODataController resides in project `Net.Appclusive.WebApi` under `OdataServices\_ENDPOINT_\_ENTITY_sController.cs`, where `_DOMAIN_` is a placeholder for the corresponding domain and `_ENDPOINT_` has to be replaced with `Core`, `Diagnostics`, ...

**Example**

```
using Net.Appclusive.Core.Domain;
using Net.Appclusive.Public.Model._DOMAIN_;

namespace Net.Appclusive.WebApi.OdataServices._ENDPOINT_
{
    [Endpoint("_ENDPOINT_")]
    public partial class _ENTITY_sController : CoreEndpointControllerBase<_ENTITY_>
    {
        public _ENTITY_sController(IEntityManager<_ENTITY_> entityManager)
            : base(entityManager)
        {
            // there is nothing to be done here
        }
    }
}
```
