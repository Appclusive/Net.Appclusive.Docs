This page contains information about onboarding processes

# Tenant Onboarding

Execute the following steps for every tenant that has to be onboarded.

1. Execute [New-Tenant.ps1](https://github.com/Appclusive/Net.Appclusive.Setup/blob/develop/src/New-Tenant.ps1)
    * `Id` = Id of the the corresponding tenant
    * `MappedId` = Id of the tenant in another system i.e. Azure
	* `Namespace` = Namespace for tenant specific products
    * `ParentId` = Id of the parent tenant. If no not provided `ParentId` will be set to `11111111-1111-1111-1111-111111111111` (SYSTEM_TENANT)

```
$Id = [guid]::Parse('399a077d-a303-4cef-972b-940eeec6e0ae');
$MappedId = [guid]::Parse('a119eb5f-6eb7-487e-b8f8-959f7ef99206');
$Name = 'NAME_OF_THE_TENANT';
$Namespace = 'Com.Example';
$ParentId = [guid]::Parse('11111111-1111-1111-1111-111111111111');

C:\PATH\TO\Net.Appclusive.Setup\src> .\New-Tenant.ps1 -Id $Id -MappedId $MappedId -Name $Name -Namespace $Namespace -ParentId $ParentId;
```
