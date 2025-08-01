+++
title = "azure_migrate_project_machine Resource"
platform = "azure"
draft = false
gh_repo = "inspec-azure"

[menu.inspec]
title = "azure_migrate_project_machine"
identifier = "inspec/resources/azure/azure_migrate_project_machine Resource"
parent = "inspec/resources/azure"
+++

Use the `azure_migrate_project_machine` InSpec audit resource to test the properties related to an Azure Migrate Project machine.

## Azure REST API Version, Endpoint, and HTTP Client Parameters

{{< readfile file="content/inspec/resources/reusable/md/inspec_azure_common_parameters.md" >}}

## Install

{{< readfile file="content/inspec/resources/reusable/md/inspec_azure_install.md" >}}

## Syntax

`resource_group`, `project_name`, and `name` are required parameters.

```ruby
describe azure_migrate_project_machine(resource_group: 'RESOURCE_GROUP', project_name: 'PROJECT_NAME', name: 'PROJECT_MACHINE_NAME') do
  it{ should exist }
  its('properties.discoveryData') { should_not be_empty }
  its('properties.discoveryData.first') { should include({ osType: 'WINDOWSGUEST' }) }
end
```

```ruby
describe azure_migrate_project_machine(resource_group: 'RESOURCE_GROUP', project_name: 'PROJECT_NAME', name: 'PROJECT_MACHINE_NAME') do
  it  { should exist }
end
```

## Parameters

`name`
: Name of the Azure Migrate Project machine to test.

`resource_group`
: Azure resource group where the targeted resource resides.

`project_name`
: Azure Migrate Assessment Project name.

The parameter set that must be provided for a valid query is `resource_group`, `project_name`, and `name`.

## Properties

`id`
: Path reference to the Migrate Project machine.

`name`
: Unique name of a Migrate Project machine.

`type`
: Type of the object. `Microsoft.Migrate/MigrateProjects/Databases`.

`properties`
: Properties of the assessment.

`properties.assessmentData`
: The assessment details of the machine published by various sources.

`properties.discoveryData`
: The discovery details of the machine published by various sources.

`properties.migrationData`
: The migration details of the machine published by various sources.

`properties.lastUpdatedTime`
: The time of the last modification of the machine.

For properties applicable to all resources, such as `type`, `name`, `id`, and `properties`, refer to [`azure_generic_resource`]({{< relref "azure_generic_resource.md#properties" >}}).

Also, see the [Azure documentation](https://docs.microsoft.com/en-us/rest/api/migrate/projects/machines/get-machine) for other available properties. 

Any attribute in the response nested within properties may be accessed with the key names separated by dots (`.`), and attributes nested in the **assessmentData** are pluralized and listed as a collection.

## Examples

### Test that the Migrate Project machine has a Windows OS

```ruby
describe azure_migrate_project_machine(resource_group: 'RESOURCE_GROUP', project_name: 'PROJECT_NAME', name: 'PROJECT_MACHINE_NAME') do
  its('properties.discoveryData.first') { should include({ osType: 'WINDOWSGUEST' }) }
end
```

## Matchers

This InSpec audit resource has the following special matchers. For a full list of available matchers, please visit our [Universal Matchers page](/inspec/matchers/).

### exists

```ruby
# If a migrate project machine is found, it will exist.

describe azure_migrate_project_machine(resource_group: 'RESOURCE_GROUP', project_name: 'PROJECT_NAME', name: 'PROJECT_MACHINE_NAME') do
  it { should exist }
end
```

### not_exists

```ruby
# If migrate project machine is not found, it will not exist.

describe azure_migrate_project_machine(resource_group: 'RESOURCE_GROUP', project_name: 'PROJECT_NAME', name: 'PROJECT_MACHINE_NAME') do
  it { should_not exist }
end
```

## Azure Permissions

{{% inspec-azure/azure_permissions_service_principal role="contributor" %}}
