+++
title = "azure_service_fabric_mesh_service Resource"
platform = "azure"
draft = false
gh_repo = "inspec-azure"

[menu.inspec]
title = "azure_service_fabric_mesh_service"
identifier = "inspec/resources/azure/azure_service_fabric_mesh_service Resource"
parent = "inspec/resources/azure"
+++

Use the `azure_service_fabric_mesh_service` InSpec audit resource to test the properties of an Azure Service Fabric Mesh service.

## Azure REST API Version, Endpoint, and HTTP Client Parameters

{{< readfile file="content/inspec/resources/reusable/md/inspec_azure_common_parameters.md" >}}

## Install

{{< readfile file="content/inspec/resources/reusable/md/inspec_azure_install.md" >}}

## Syntax

```ruby
describe azure_service_fabric_mesh_service(resource_group: 'RESOURCE_GROUP', name: 'SERVICE_FABRIC_MESH_SERVICE_NAME') do
  it                                      { should exist }
  its('type')                             { should eq 'Microsoft.ServiceFabricMesh/applications' }
end
```

```ruby
describe azure_service_fabric_mesh_service(resource_group: 'RESOURCE_GROUP', name: 'SERVICE_FABRIC_MESH_SERVICE_NAME') do
  it  { should exist }
end
```

## Parameters

`name` _(required)_
: Name of the Azure Service Fabric Mesh service to test.

`resource_group` _(required)_
: Azure resource group where the targeted resource resides.

## Properties

`id`
: Resource ID.

`name`
: Resource name.

`type`
: Resource type. `Microsoft.ServiceFabricMesh/services`.

`properties`
: The properties of the **Service Fabric Mesh Service**.

`properties.osType`
: The Operating system type required by the code in service.

`properties.replicaCount`
: The number of replicas of the service to create. Defaults to 1 if not specified.

`properties.healthState`
: Describes the health state of a services resource.

For properties applicable to all resources, such as `type`, `name`, `id`, and `properties`, refer to [`azure_generic_resource`]({{< relref "azure_generic_resource.md#properties" >}}).

Also, see the [Azure documentation](https://docs.microsoft.com/en-us/rest/api/servicefabric/sfmeshrp-api-service_get) for other available properties.

## Examples

### Test that the 'Service Fabric Mesh Service' is healthy

```ruby
describe azure_service_fabric_mesh_service(resource_group: 'RESOURCE_GROUP', name: 'SERVICE_FABRIC_MESH_SERVICE_NAME') do
  its('properties.healthState') { should eq 'Ok' }
end
```

## Matchers

{{< readfile file="content/inspec/reusable/md/inspec_matchers_link.md" >}}

This resource has the following special matchers.

### exists

```ruby
# If a Service Fabric Mesh Service is found, it will exist.

describe azure_service_fabric_mesh_service(resource_group: 'RESOURCE_GROUP', name: 'SERVICE_FABRIC_MESH_SERVICE_NAME') do
  it { should exist }
end
```

### not_exists

```ruby
# If Service Fabric Mesh Service is not found, it will not exist.

describe azure_service_fabric_mesh_service(resource_group: 'RESOURCE_GROUP', name: 'SERVICE_FABRIC_MESH_SERVICE_NAME') do
  it { should_not exist }
end
```

## Azure Permissions

{{% inspec-azure/azure_permissions_service_principal role="reader" %}}
