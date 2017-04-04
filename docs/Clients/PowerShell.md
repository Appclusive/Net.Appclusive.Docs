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
		apiBaseUri="http://appclusive.example.com/api"
		credential="admin|p@ssw0rd"
	/>

## JWT Authentication

	<net_Appclusive_PS_Client
		apiBaseUri="http://appclusive.example.com/api"
		credential="[bearer@auth.appclusive.net]|JWT_TOKEN_HERE"
	/>


# Enter Server


