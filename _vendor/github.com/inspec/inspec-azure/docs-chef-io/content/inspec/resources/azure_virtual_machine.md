+++
title = "azure_virtual_machine Resource"
platform = "azure"
draft = false
gh_repo = "inspec-azure"

[menu.inspec]
title = "azure_virtual_machine"
identifier = "inspec/resources/azure/azure_virtual_machine Resource"
parent = "inspec/resources/azure"
+++

Use the `azure_virtual_machine` InSpec audit resource to test the properties related to a virtual machine.

## Azure REST API Version, Endpoint, and HTTP Client Parameters

{{< readfile file="content/inspec/resources/reusable/md/inspec_azure_common_parameters.md" >}}

## Install

{{< readfile file="content/inspec/resources/reusable/md/inspec_azure_install.md" >}}

## Syntax

`resource_group` and virtual machine `name`, or the `resource_id` are required parameters.

```ruby
describe azure_virtual_machine(resource_group: 'RESOURCE_GROUP', name: 'VM_NAME') do
  it { should exist }
end
```

```ruby
describe azure_virtual_machine(resource_id: '/subscriptions/{subscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.Compute/virtualMachines/{vmName}') do
  it { should exist }
end
```

## Parameters

`resource_group`
: Azure resource group where the targeted resource resides.

`name`
: Name of the Azure resource to test.

`resource_id`
: The unique resource ID.

Either one of the parameter sets can be provided for a valid query:

- `resource_id`
- `resource_group` and `name`

## Properties

`admin_username`
: The admin user name.

`resources`
: The virtual machine child extension resources.

`zones`
: The virtual machine's availability zones. `its('zones') should include('zone1', 'zone2')`.

`installed_extensions_types`
: List of all installed extensions' types for the virtual machine. `its('installed_extensions_types') { should include('ExtensionType') }`.

`installed_extensions_names`
: List of all installed extensions' names for the virtual machine. `its('installed_extensions_names') { should include('ExtensionName') }`.

`has_monitoring_agent_installed?`
: Indicates whether a monitoring agent is installed.

`has_endpoint_protection_installed?`
: Indicates whether a list of endpoint protection extension types are installed. `it { should have_endpoint_protection_installed(%w{ep_type_1 ep_type_2}) }`.

`has_only_approved_extensions?`
: Indicates whether only provided extension types are installed. `it { should have_only_approved_extensions(%w{extension_type_1 extension_type_2}) }`.

`os_disk_name`
: The virtual machine's operating system disk name. `its('os_disk_name') { should cmp 'OsDiskName' }`.

`data_disk_names`
: The virtual machine's data disk names. `its('data_disk_names') { should include('DataDisk1') }`.

For properties applicable to all resources, such as `type`, `name`, `id`, and `properties`, refer to [`azure_generic_resource`]({{< relref "azure_generic_resource.md#properties" >}}).

Also, see the [Azure documentation](https://docs.microsoft.com/en-us/rest/api/compute/virtualmachines/get#virtualmachine) for other available properties. You can access any attribute in the response with the key names separated by dots (`.`).

## Examples

### Ensure that the virtual machine has the expected data Disks

```ruby
describe azure_virtual_machine(resource_group: 'MyResourceGroup', name: 'MyVmName') do
  its('data_disk_names') { should include('DataDisk1') }
end
```

**Ensure that the Virtual Machine has the Expected Monitoring Agent Installed.**

```ruby
describe azure_virtual_machine(resource_group: 'MyResourceGroup', name: 'MyVmName') do
  it { should have_monitoring_agent_installed }
end
```

## Matchers

This InSpec audit resource has the following special matchers. For a full list of available matchers, please visit our [Universal Matchers page](/inspec/matchers/).

### exists

```ruby
# If a virtual machine is found, it will exist.

describe azure_virtual_machine(resource_group: 'RESOURCE_GROUP', name: 'VM_NAME') do
  it { should exist }
end

# virtual machines that are not found, will not exist.

describe azure_virtual_machine(resource_group: 'RESOURCE_GROUP', name: 'VM_NAME') do
  it { should_not exist }
end
```

### have_only_approved_extensions

```ruby
# Check if a virtual machine has only approved extensions. The check will fail if an extension is used that's not on the list.

describe azure_virtual_machine(resource_group: 'RESOURCE_GROUP', name: 'VM_NAME') do
  it { should have_only_approved_extensions(['ApprovedExtension', 'OtherApprovedExtensions']) }
end
```

### have_monitoring_agent_installed

```ruby
# Will be true if the MicrosoftMonitoringAgent is installed (Windows only).

describe azure_virtual_machine(resource_group: 'MyResourceGroup', name: 'MyVmName') do
  it { should have_monitoring_agent_installed }
end
```

### have_endpoint_protection_installed

```ruby
# Will be true if any of the given extensions are installed.

describe azure_virtual_machine(resource_group: 'RESOURCE_GROUP', name: 'VM_NAME') do
  it { should have_endpoint_protection_installed(['Extension1', 'Extension2']) }
end
```

## Azure Permissions

{{% inspec-azure/azure_permissions_service_principal role="contributor" %}}
