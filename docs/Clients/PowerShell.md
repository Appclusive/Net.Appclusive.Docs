Appclusive (R) provides a Microsoft PowerShell module which you can use to manage and work with the Appclusive API. This module is built on the [C# .NET Client](CSharpDotNet/) and acts as a convenience wrapper. For more information about the [C# .NET Client](CSharpDotNet/) see the documentation [.Net C# Client](../CSharpDotNet/).

# Installation

**Prerequisites:**

* PowerShell 5

The PowerShell client module is available on [NuGet](https://www.nuget.org/packages/Net.Appclusive.PS.Client/) and on [PowerShellGallery](https://www.powershellgallery.com/packages/Net.Appclusive.PS.Client)

## Install from NuGet

To install the most recent version of the PowerShell client module from NuGet execute the following steps.

1. Open PowerShell
1. Install [Net.Appclusive.PS.Client](https://www.nuget.org/packages/Net.Appclusive.PS.Client/) from [NuGet](https://www.nuget.org/)

	`PATH\TO\nuget.exe install Net.Appclusive.PS.Client`

1. Execute `PATH\TO\Net.Appclusive.PS.Client\Install.ps1`

## Install from PowerShellGallery

To install the most recent version of the PowerShell client module from PowerShellGallery execute the following steps.

1. Open PowerShell
1. Execute `Install-Module -Name Net.Appclusive.PS.Client`

## Install from Project

To install the PowerShell client module during development without releasing on [NuGet](https://www.nuget.org/) execute the following commands in PowerShell console.

```PoSH
# change to Downloads directory of current user
$downloadsDirectory = "$env:USERPROFILE\Downloads";
cd $downloadsDirectory;
ls -Recurse * | Remove-Item -Recurse -Force;

# install dependent NuGet packages
nuget install biz.dfch.CS.Commons -version 1.12.0;
nuget install biz.dfch.CS.PowerShell.Commons -version 2.0.0;
nuget install Microsoft.Data.Edm -version 5.6.4;
nuget install Microsoft.Data.OData -version 5.6.4;
nuget install Microsoft.Data.Services.Client -version 5.6.4;
nuget install System.Linq.Dynamic -version 1.0.7;
nuget install System.Spatial -version 5.6.4;

# create and install NuGet package Net.Appclusive.PS.Client
nuget pack C:\src\Net.Appclusive.Net.Client\src\Net.Appclusive.PS.Client.nuspec;
nuget install Net.Appclusive.PS.Client -Source $downloadsDirectory -OutputDirectory packages;
.\packages\Net.Appclusive.PS.Client.4.0.0\Install.ps1;
```

# Configuration

The Appclusive API base URI and the credential to be used by the PowerShell client can be specified in the configuration file of the PowerShell client module.

`C:\Program Files\WindowsPowerShell\Modules\Net.Appclusive.PS.Client\Net.Appclusive.PS.Client.config`

## Basic Authentication

Username without domain

	<net_Appclusive_PS_Client
		apiBaseUri="http://appclusive/api"
		credential="Arbitrary|P@ssw0rd"
	/>

Username with domain

	<net_Appclusive_PS_Client
		apiBaseUri="http://appclusive/api"
		credential="MyDomain\Arbitrary|P@ssw0rd"
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

    * Required parameters
		* `ApiBaseUri`
		* `Username`
		* `Password` (Use `Username` = `bearer@auth.appclusive.net` for `Bearer` authentication)
    * Optional parameters
		* `TenantId`

* credential

    * Required parameters
		* `ApiBaseUri`
		* `Credential` (Use `bearer@auth.appclusive.net` as username of the credential for `Bearer` authentication)
    * Optional parameters
		* `TenantId`
  
* config

    * Required parameters
		* `UseModuleContext`
    * Optional parameters
		* `TenantId`
  
Invoking `Enter-Server` with the `UseModuleContext` switch implies that the Cmdlet `ApiBaseUri` and `Credential` will be loaded from the config file (`C:\Program Files\WindowsPowerShell\Modules\Net.Appclusive.PS.Client\Net.Appclusive.PS.Client.config`).

### Code Samples

```
Import-Module Net.Appclusive.PS.Client

$svc = Enter-ApcServer -ApiBaseUri 'http://appclusive/api' -Username Arbitrary -Password P@ssw0rd;
$svc = Enter-ApcServer -ApiBaseUri 'http://appclusive/api' -Username "Mydomain\Arbitrary" -Password P@ssw0rd;
$svc = Enter-ApcServer -ApiBaseUri 'http://appclusive/api' -Credential Get-Credential;
$svc = Enter-ApcServer -UseModuleContext;
```

## Get Cmdlets

The Get Cmdlets can be used to retrieve available entities of the corresponding entity set or to get specific entities by `Id` or `Name`.

### Code Samples

```
Import-Module Net.Appclusive.PS.Client

$svc = Enter-ApcServer -ApiBaseUri 'http://appclusive/api' -Username Arbitrary -Password P@ssw0rd;

Get-ApcTenant

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

Get-ApcTenant -Id 11111111-1111-1111-1111-111111111111


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

Get-ApcTenant -Name SYSTEM_TENANT


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

## New Cmdlets

The New Cmdlets can be used to create entities of the corresponding entity set.

### Code Samples

```
Import-Module Net.Appclusive.PS.Client

$svc = Enter-ApcServer -ApiBaseUri 'http://appclusive/api' -Username Arbitrary -Password P@ssw0rd;

New-ApcAcl -Name ArbitraryAcl -ParentId 1 -Svc $svc;

ParentId      : 1
NoInheritance : False
Id            : 13
Name          : ArbitraryAcl
Description   :
Parent        :
Children      : {}
Aces          : {}
Details       :
```

## Utility Methods

The `Net.Appclusive.PS.Client` provides some C# utilty methods. These methods are extension methods and are not yet exposed as Cmdlets. The extension methods are there to simplify the usage of the Data Service Context.


### Code Samples

```
Import-Module Net.Appclusive.PS.Client

$svc = Enter-ApcServer -ApiBaseUri 'http://appclusive/api' -Username Arbitrary -Password P@ssw0rd;

$filterQuery = "Name eq 'TestUser'";
[Net.Appclusive.Api.DataServiceQueryExtensions]::Filter($svc.Core.Users, $filterQuery);
[Net.Appclusive.Api.DataServiceQueryExtensions]::Id($svc.Core.Users, 1);
```
