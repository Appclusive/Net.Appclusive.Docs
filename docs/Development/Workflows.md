# Development

## Setup Persistence Database

To create the persistence database execute the following steps.

1. Open `SQL Server Object Explorer` in Visual Studio 2015
1. Expand `SQL Server` and `(localdb)\MSSQLLocalDB (SQL Server 13.0.2151 - COMPUTERNAME\USERNAME)`
1. Right click on `Databases`
  1. Select `Add New Database` from context menu
    1. Database Name: `AppclusiveWorkflows`
    1. Database Location: `C:\src\App_Data`
1. Right click on newly created database `AppclusiveWorkflows`
  1. Select `New Query` from context menu
  1. Copy content of `C:\Windows\Microsoft.NET\Framework\4.0.30319\sql\en\SqlWorkflowInstanceStoreSchema.sql` to the query windows and execute query
  1. Copy content of `C:\Windows\Microsoft.NET\Framework\4.0.30319\sql\en\SqlWorkflowInstanceStoreLogic.sql` to the query windows and execute query

## Design for an Activity

The following blog post explains how to create a design for an Activity.

[Associating a WF4 activity designer to a custom activity using MetadataStore](http://geekswithblogs.net/jkurtz/archive/2010/01/26/137639.aspx)

### Important

The following points have to be considered when creating a design for an Activity

* In order for Visual Studio activity designer to present the designer for `ArbitraryActivity` the `ArbitraryActivity.Design.dll` has to be in the same folder as `ArbitraryActivity.dll`
* The project containing the workflow the `ArbitraryActivity` will be used in has to have references to both projects, the `ArbitraryActivity` project and the `ArbitraryActivity.Design` project.

## Restrictions

* An Activity cannot contain parts of the project namespace in its name (i.e. an `Activity` with name `Net.Appclusive.Workflows.ArbitraryActivity` will not compile, if the `ArbitraryActivity` resides in `Net.Appclusive.SomeProject`). Naming an `Activity` like `NetAppclusiveWorkflowsArbitraryActivity` is not a problem.
* When invoking an `Activity` based on its XAML definition the `Activity` first has to be loaded using `ActivityXamlServices`. Loading the `Activity` results in a instance of type `DynamicActivity`. This means, that the type of the `Activity` cannot be determined anymore. Because of that `WorkflowManagers` invoke method has to be called by passing the instance of the loaded `Activity` instead of the type. Otherwise the `WorkflowManager` will instantiate a new `DynamicActivity` without any Properties, Variables and Arguments.

# Useful Links

* [Getting started](https://code.msdn.microsoft.com/windowsapps/Windows-Workflow-deed2cd5)
* [Howto create a Workflow](https://msdn.microsoft.com/en-us/library/dd489437(VS.110).aspx)
* [Howto run a Workflow](https://msdn.microsoft.com/en-us/library/dd489463(VS.110).aspx)
* [Howto create an Activity](https://msdn.microsoft.com/en-us/library/dd489453(VS.110).aspx)
* [Associating a WF4 Activity designer to a custom Activity using MetadataStore](http://geekswithblogs.net/jkurtz/archive/2010/01/26/137639.aspx)
* [Workflow Persistence](https://msdn.microsoft.com/en-us/library/dd489420(v=vs.110).aspx)
* [Howto enable Persistence for Workflows and Workflow Services](https://msdn.microsoft.com/en-us/library/ee829476(v=vs.110).aspx)
