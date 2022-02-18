+++
title = "UnnecessaryOSCheck"

+++

<!-- This content is automatically generated. See https://github.com/chef/chef-web-docs/blob/main/generated/README.md -->

The department is: `UnnecessaryOSCheck`

The full name of the cop is: `Chef/Style/UnnecessaryOSCheck`

| Enabled by default | Supports autocorrection | Target Chef Version |
| --- | --- | --- |
| Enabled | Yes | All Versions |

## Examples


#### incorrect

```ruby
node['os'] == 'darwin'
node['os'] == 'windows'
node['os'].eql?('aix')
%w(netbsd openbsd freebsd).include?(node['os'])
```

#### correct

```ruby
platform_family?('mac_os_x')
platform_family?('windows')
platform_family?('aix')
platform_family?('netbsd', 'openbsd', 'freebsd)
```

## Configurable attributes

<table>
<tbody><tr>
<th>Name</th>
<th>Default value</th>
<th>Configurable values</th>
</tr>
<tr>
<td style="text-align:center">Version Added</td>
<td style="text-align:center">5.21.0</td>
<td style="text-align:center">String</td>
</tr>
<tr><td style="text-align:center">Include</td>
<td style="text-align:center"><ul>
</ul>
</td>
<td style="text-align:center">Array</td>
</tr></tbody></table>