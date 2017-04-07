Appclusive (R) provides a Microsoft PowerShell module which you can use to manage and work with the Appclusive API. This module is built on the DotNetCSharp client and acts as a convenience wrapper. For more information about the DotNetCSharp client see the documentation [.Net C# client](../DotNetCSharp/!index).


# Installation

**Prerequisites:**

* PowerShell 5

The PowerShell client module is available on [NuGet](https://www.nuget.org/packages/Net.Appclusive.PS.Client/) and on [PowerShellGallery](https://www.powershellgallery.com/packages/Net.Appclusive.PS.Client)

## Install from NuGet

To install the PowerShell client module from NuGet execute the following steps.

1. Open PowerShell
1. Download [Net.Appclusive.PS.Client](https://www.nuget.org/packages/Net.Appclusive.PS.Client/) from [NuGet](https://www.nuget.org/)

	`PS> PATH\TO\nuget.exe install Net.Appclusive.PS.Client`

1. Execute `PS> PATH\TO\Net.Appclusive.PS.Client\Install.ps1`

## Install from PowerShellGallery

To install the PowerShell client module from PowerShellGallery execute the following steps.

1. Open PowerShell
1. Execute `PS> Install-Module -Name Net.Appclusive.PS.Client`

# Configuration

The Appclusive API base URI and the credential to be used by the PowerShell client can be specified in the configuration file of the PowerShell client module.

`C:\Program Files\WindowsPowerShell\Modules\Net.Appclusive.PS.Client\Net.Appclusive.PS.Client.config`

## Basic Authentication

	<net_Appclusive_PS_Client
		apiBaseUri="http://appclusive/api"
		credential="Arbitrary|p@ssw0rd"
	/>

## JWT Authentication

	<net_Appclusive_PS_Client
		apiBaseUri="http://appclusive/api"
		credential="bearer@auth.appclusive.net|JWT_HERE"
	/>


# Usage

## Login (Enter-Server)

The `Enter-Server` Cmdlet performs a login to an Appclusive server. This is the first Cmdlet to be executed and required for all other Cmdlets of this module. It creates service references to the routers of the application. The `Enter-Server` Cmdlet defines the following parameter sets. 

* plain

    * Required parameters: `ApiBaseUri`, `Username` `Password` (Use `Username` = `bearer@auth.appclusive.net` for `Bearer` authentication)
    * Optional parameters: `TenantId`

* credential

    * Required parameters: `ApiBaseUri`, `Credential` (Use `bearer@auth.appclusive.net` as username of the credential for `Bearer` authentication)
    * Optional parameters: `TenantId`
  
* config

    * Required parameters: `UseModuleContext`
    * Optional parameters: `TenantId`
  
Invoking `Enter-Server` with the `UseModuleContext` switch implies that the Cmdlet `ApiBaseUri` and `Credential` will be loaded from the config file (`C:\Program Files\WindowsPowerShell\Modules\Net.Appclusive.PS.Client\Net.Appclusive.PS.Client.config`).

### Code Samples

```
PS> $svc = Enter-ApcServer -ApiBaseUri 'http://appclusive/api' -Username Arbitrary -Password p@ssw0rd;
PS> $svc = Enter-ApcServer -ApiBaseUri 'http://appclusive/api' -Credential Get-Credential;
PS> $svc = Enter-ApcServer -UseModuleContext;
```

## Get Cmdlets

The Get Cmdlets can be used to retrieve available entities of the corresponding entity set or to get spcific entities by `Id` or `Name`.

### Code Samples

```
PS> $svc = Enter-ApcServer -ApiBaseUri 'http://appclusive/api' -Username Arbitrary -Password p@ssw0rd;
PS> Get-ApcTenant

Id          : 11111111-1111-1111-1111-111111111111
Name        : SYSTEM_TENANT
Description : SYSTEM_TENANT
MappedId    : 11111111-1111-1111-1111-111111111111
MappedType  : Internal
ParentId    : 11111111-1111-1111-1111-111111111111
Namespace   : Net.Appclusive
CustomerId  : 0
Details     :
Parent      :
Children    : {}

PS> Get-ApcTenant -Id 11111111-1111-1111-1111-111111111111


Id          : 11111111-1111-1111-1111-111111111111
Name        : SYSTEM_TENANT
Description : SYSTEM_TENANT
MappedId    : 11111111-1111-1111-1111-111111111111
MappedType  : Internal
ParentId    : 11111111-1111-1111-1111-111111111111
Namespace   : Net.Appclusive
CustomerId  : 0
Details     :
Parent      :
Children    : {}

PS> Get-ApcTenant -Name SYSTEM_TENANT


Id          : 11111111-1111-1111-1111-111111111111
Name        : SYSTEM_TENANT
Description : SYSTEM_TENANT
MappedId    : 11111111-1111-1111-1111-111111111111
MappedType  : Internal
ParentId    : 11111111-1111-1111-1111-111111111111
Namespace   : Net.Appclusive
CustomerId  : 0
Details     :
Parent      :
Children    : {}
```
