+++
title = "azure_container_group Resource"
platform = "azure"
draft = false
gh_repo = "inspec-azure"

[menu.inspec]
title = "azure_container_group"
identifier = "inspec/resources/azure/azure_container_group Resource"
parent = "inspec/resources/azure"
+++

Use the `azure_container_group` InSpec audit resource to test the properties related to an Azure container group.

## Azure REST API Version, Endpoint, and HTTP Client Parameters

{{< readfile file="content/inspec/resources/reusable/md/inspec_azure_common_parameters.md" >}}

## Install

{{< readfile file="content/inspec/resources/reusable/md/inspec_azure_install.md" >}}

## Syntax

`name` is a required parameter, and `resource_group` could be provided as an optional parameter.

```ruby
describe azure_container_group(resource_group: 'RESOURCE_GROUP_NAME', name: 'CONTAINER_GROUP_NAME') do
  it                                      { should exist }
  its('name')                             { should cmp 'demo1' }
  its('type')                             { should cmp 'Microsoft.ContainerInstance/containerGroups' }
  its('location')                         { should cmp 'WestUs'}
end
```

```ruby
describe azure_container_group(resource_group: 'RESOURCE_GROUP_NAME', name: 'CONTAINER_GROUP_NAME') do
  it  { should exist }
end
```

## Parameters

`name`
: Name of the Azure container group to test.

`resource_group`
: Azure resource group where the targeted resource resides.

The parameter sets that should be provided for a valid query are `resource_group` and `name`.

## Properties

`id`
: The resource ID.

`name`
: The container group name.

`type`
: The resource type.

`location`
: The resource location.

`properties`
: The properties of the resource.

For properties applicable to all resources, such as `type`, `name`, `id`, and `properties`, refer to [`azure_generic_resource`]({{< relref "azure_generic_resource.md#properties" >}}).

Also, see the [Azure documentation](https://docs.microsoft.com/en-us/rest/api/container-instances/container-groups/get) for other available properties. You can access any attribute in the response with the key names separated by dots (`.`).

## Examples

### Test that the container group has a public IP address

```ruby
describe azure_container_group(resource_group: 'RESOURCE_GROUP_NAME', name: 'CONTAINER_GROUP_NAME') do
  its('properties.ipAddress.type') { should eq 'Public'}
end
```

## Matchers

This InSpec audit resource has the following special matchers. For a full list of available matchers, please visit our [Universal Matchers page](/inspec/matchers/).

### exists

```ruby
# If a container group is found, it will exist.

describe azure_container_group(resource_group: 'RESOURCE_GROUP_NAME', name: 'CONTAINER_GROUP_NAME') do
  it { should exist }
end
```

### not_exists

```ruby
# container groups that are not found, will not exist.
describe azure_container_group(resource_group: 'RESOURCE_GROUP_NAME', name: 'CONTAINER_GROUP_NAME') do
  it { should_not exist }
end
```

## Azure Permissions

{{% inspec-azure/azure_permissions_service_principal role="contributor" %}}
