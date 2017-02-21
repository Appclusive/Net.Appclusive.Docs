This page contains information about the setup of the development environment

* Checkout repository [Net.Appclusive](https://github.com/dfensgmbh/biz.dfch.CS.ProductEngine)
* Open solution in Visual Studio 2015
* Build solution
* Import strong name key according blog post [`Use Strong Name Key on TeamCity for Digital Signature`](https://d-fens.ch/2016/10/18/use-strong-name-key-on-teamcity-for-digital-signature/)
* Rebuild solution
* Setup file database
    * Open `Package Manager Console` in Visual Studio 2015
    * Select `Net.Appclusive.Core` as `Default Project`
	* Execute `Update-Database`
	* Execute [`Add-ApcSpecialEntities.ps1`](https://github.com/Appclusive/Net.Appclusive.Setup/blob/develop/src/Add-ApcSpecialEntities.ps1) in PowerShell console as follows
	
	    `.\Add-ApcSpecialEntities.ps1 -ConnectionType LocalDB`

* To add some demo data execute [`Add-ApcExampleData.ps1`](https://github.com/Appclusive/Net.Appclusive.Setup/blob/develop/src/Add-ApcExampleData.ps1) in PowerShell console as follows

		`.\Add-ApcExampleData.ps1 -ConnectionType LocalDB`
