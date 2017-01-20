This page contains information about the setup of the development environment

1. Checkout repository [Net.Appclusive](https://github.com/dfensgmbh/biz.dfch.CS.ProductEngine)
1. Open solution in Visual Studio 2015
1. Build solution
1. Import strong name key according blog post [`Use Strong Name Key on TeamCity for Digital Signature`](https://d-fens.ch/2016/10/18/use-strong-name-key-on-teamcity-for-digital-signature/)
1. Setup file database
    1. Open `Package Manager Console` in Visual Studio 2015
    1. Select `Net.Appclusive.Core` as `Default Project`
	1. Execute `Update-Database`
