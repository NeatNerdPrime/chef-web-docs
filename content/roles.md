+++
title = "About Roles"
draft = false
gh_repo = "chef-web-docs"
aliases = ["/roles.html"]
product = ["client", "server"]

[menu]
  [menu.infra]
    title = "Roles"
    identifier = "chef_infra/policyfiles/roles.md Roles"
    parent = "chef_infra/policyfiles"
    weight = 70
+++
<!-- markdownlint-disable-file MD033 -->
{{< readfile file="content/reusable/md/role.md" >}}

## Role Attributes

{{< note >}}

{{< readfile file="content/reusable/md/notes_see_attributes_overview.md" >}}

{{< /note >}}

{{< readfile file="content/reusable/md/role_attribute.md" >}}

### Attribute Types

There are two types of attributes that can be used with roles:

<table>
<colgroup>
<col style="width: 40%" />
<col style="width: 60%" />
</colgroup>
<thead>
<tr class="header">
<th>Attribute Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td><code>default</code></td>
<td>{{< readfile file="content/reusable/md/node_attribute_type_default.md" >}}</td>
</tr>
<tr>
<td><code>override</code></td>
<td>{{< readfile file="content/reusable/md/node_attribute_type_override.md" >}}</td>
</tr>
</tbody>
</table>

## Role Formats

Role data is stored in two formats: as a Ruby file that contains
domain-specific language or as JSON data.

### Chef Language

{{< readfile file="content/reusable/md/ruby_summary.md" >}}

Domain-specific Ruby attributes:

<table>
<colgroup>
<col style="width: 40%" />
<col style="width: 60%" />
</colgroup>
<thead>
<tr class="header">
<th>Setting</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td><p><code>default_attributes</code></p></td>
<td><p>Optional. A set of attributes to be applied to all nodes, assuming the node doesn't already have a value for the attribute. This is useful for setting global defaults that can then be overridden for specific nodes. If more than one role attempts to set a default value for the same attribute, the last role applied is the role to set the attribute value. When nested attributes are present, they're preserved. For example, to specify that a node that has the attribute <code>apache2</code> should listen on ports 80 and 443 (unless ports are already specified):</p>
<div class="sourceCode" id="cb1"><pre class="sourceCode ruby"><code class="sourceCode ruby"><span id="cb1-1"><a href="#cb1-1"></a>default_attributes <span class="st">&#39;apache2&#39;</span> =&gt; {</span>
<span id="cb1-2"><a href="#cb1-2"></a>  <span class="st">&#39;listen_ports&#39;</span> =&gt; [ <span class="st">&#39;80&#39;</span>, <span class="st">&#39;443&#39;</span> ]</span>
<span id="cb1-3"><a href="#cb1-3"></a>}</span></code></pre></div></td>
</tr>
<tr>
<td><p><code>description</code></p></td>
<td><p>A description of the functionality that's covered. For example:</p>
<div class="sourceCode" id="cb2"><pre class="sourceCode ruby"><code class="sourceCode ruby"><span id="cb2-1"><a href="#cb2-1"></a>description <span class="st">&#39;The base role for systems that serve HTTP traffic&#39;</span></span></code></pre></div></td>
</tr>
<tr>
<td><p><code>env_run_lists</code></p></td>
<td><p>Optional. A list of environments, each specifying a recipe or a role to be applied to that environment. This setting must specify the <code>_default</code> environment. If the <code>_default</code> environment is set to <code>[]</code> or <code>nil</code>, then the run-list is empty. For example:</p>
<div class="sourceCode" id="cb3"><pre class="sourceCode ruby"><code class="sourceCode ruby"><span id="cb3-1"><a href="#cb3-1"></a>env_run_lists <span class="st">&#39;prod&#39;</span> =&gt; [<span class="st">&#39;recipe[apache2]&#39;</span>],</span>
<span id="cb3-2"><a href="#cb3-2"></a>              <span class="st">&#39;staging&#39;</span> =&gt; [<span class="st">&#39;recipe[apache2::staging]&#39;</span></span></code></pre></div>
{{< warning >}}
<p>Using <code>env_run_lists</code> with roles is discouraged as it can be difficult to maintain over time. Instead, consider using multiple roles to define the required behavior.</p>
{{< /warning >}}</td>
</tr>
<tr>
<td><p><code>name</code></p></td>
<td><p>A unique name within the organization. Each name must be made up of letters (uppercase and lowercase), numbers, underscores, and hyphens: [A-Z][a-z][0-9] and [_-]. Spaces aren't allowed. For example:</p>
<div class="sourceCode" id="cb4"><pre class="sourceCode ruby"><code class="sourceCode ruby"><span id="cb4-1"><a href="#cb4-1"></a>name <span class="st">&#39;dev01-24&#39;</span></span></code></pre></div></td>
</tr>
<tr>
<td><p><code>override_attributes</code></p></td>
<td><p>Optional. A set of attributes to be applied to all nodes, even if the node already has a value for an attribute. This is useful for ensuring that certain attributes always have specific values. If more than one role attempts to set an override value for the same attribute, the last role applied wins. When nested attributes are present, they're preserved. For example:</p>
<div class="sourceCode" id="cb5"><pre class="sourceCode ruby"><code class="sourceCode ruby"><span id="cb5-1"><a href="#cb5-1"></a>override_attributes <span class="st">&#39;apache2&#39;</span> =&gt; {</span>
<span id="cb5-2"><a href="#cb5-2"></a>  <span class="st">&#39;max_children&#39;</span> =&gt; <span class="st">&#39;50&#39;</span></span>
<span id="cb5-3"><a href="#cb5-3"></a>}</span></code></pre></div>
<p>The parameters in a Ruby file are Ruby method calls, so parentheses can be used to provide clarity when specifying numerous or deeply-nested attributes. For example:</p>
<div class="sourceCode" id="cb6"><pre class="sourceCode ruby"><code class="sourceCode ruby"><span id="cb6-1"><a href="#cb6-1"></a>override_attributes(</span>
<span id="cb6-2"><a href="#cb6-2"></a>  <span class="st">:apache2</span> =&gt; {</span>
<span id="cb6-3"><a href="#cb6-3"></a>    <span class="st">:prefork</span> =&gt; { <span class="st">:min_spareservers</span> =&gt; <span class="ch">&#39;5&#39;</span> }</span>
<span id="cb6-4"><a href="#cb6-4"></a>  }</span>
<span id="cb6-5"><a href="#cb6-5"></a>)</span></code></pre></div>
<p>Or:</p>
<div class="sourceCode" id="cb7"><pre class="sourceCode ruby"><code class="sourceCode ruby"><span id="cb7-1"><a href="#cb7-1"></a>override_attributes(</span>
<span id="cb7-2"><a href="#cb7-2"></a>  <span class="st">:apache2</span> =&gt; {</span>
<span id="cb7-3"><a href="#cb7-3"></a>    <span class="st">:prefork</span> =&gt; { <span class="st">:min_spareservers</span> =&gt; <span class="ch">&#39;5&#39;</span> }</span>
<span id="cb7-4"><a href="#cb7-4"></a>  },</span>
<span id="cb7-5"><a href="#cb7-5"></a>  <span class="st">:tomcat</span> =&gt; {</span>
<span id="cb7-6"><a href="#cb7-6"></a>    <span class="st">:worker_threads</span> =&gt; <span class="st">&#39;100&#39;</span></span>
<span id="cb7-7"><a href="#cb7-7"></a>  }</span>
<span id="cb7-8"><a href="#cb7-8"></a>)</span></code></pre></div></td>
</tr>
<tr>
<td><p><code>run_list</code></p></td>
<td><p>A list of recipes and/or roles to be applied and the order in which they're to be applied. For example, the following run-list:</p>
<div class="sourceCode" id="cb8"><pre class="sourceCode ruby"><code class="sourceCode ruby"><span id="cb8-1"><a href="#cb8-1"></a>run_list <span class="st">&#39;recipe[apache2]&#39;</span>,</span>
<span id="cb8-2"><a href="#cb8-2"></a>         <span class="st">&#39;recipe[apache2::mod_ssl]&#39;</span>,</span>
<span id="cb8-3"><a href="#cb8-3"></a>         <span class="st">&#39;role[monitor]&#39;</span></span></code></pre></div>
<p>would apply the <code>apache2</code> recipe first, then the <code>apache2::mod_ssl</code> recipe, and then the <code>role[monitor]</code> recipe.</p></td>
</tr>
</tbody>
</table>

Each role must be saved as a ruby file in the `roles/` subdirectory of
the chef-repo. (If the repository doesn't have this subdirectory, then
create it using knife.) Each Ruby file should have the `.rb` suffix. A
complete role has the following syntax:

```ruby
name "role_name"
description "role_description"
run_list "recipe[name]", "recipe[name::attribute]", "recipe[name::attribute]"
env_run_lists "name" => ["recipe[name]"], "environment_name" => ["recipe[name::attribute]"]
default_attributes "node" => { "attribute" => [ "value", "value", "etc." ] }
override_attributes "node" => { "attribute" => [ "value", "value", "etc." ] }
```

where both default and override attributes are optional and at least one
run-list (with at least one run-list item) is specified. For example, a
role named `webserver` that has a run-list that defines actions for
three different roles, and for certain roles takes extra steps (such as
the `apache2` role listening on ports 80 and 443):

```ruby
name "webserver"
description "The base role for systems that serve HTTP traffic"
run_list "recipe[apache2]", "recipe[apache2::mod_ssl]", "role[monitor]"
env_run_lists "prod" => ["recipe[apache2]"], "staging" => ["recipe[apache2::staging]"], "_default" => []
default_attributes "apache2" => { "listen_ports" => [ "80", "443" ] }
override_attributes "apache2" => { "max_children" => "50" }
```

### JSON

The JSON format for roles maps directly to the domain-specific Ruby
format: same settings, attributes, and values, and a similar structure
and organization. For example:

```json
{
  "name": "webserver",
  "chef_type": "role",
  "json_class": "Chef::Role",
  "default_attributes": {
    "apache2": {
      "listen_ports": [
        "80",
        "443"
      ]
    }
  },
  "description": "The base role for systems that serve HTTP traffic",
  "run_list": [
    "recipe[apache2]",
    "recipe[apache2::mod_ssl]",
    "role[monitor]"
  ],
  "env_run_lists" : {
    "production" : [],
    "preprod" : [],
    "dev": [
      "role[base]",
      "recipe[apache]",
      "recipe[apache::copy_dev_configs]",
    ],
    "test": [
      "role[base]",
      "recipe[apache]"
    ]
  },
  "override_attributes": {
    "apache2": {
      "max_children": "50"
    }
  }
}
```

The JSON format has two additional settings:

<table>
<colgroup>
<col style="width: 40%" />
<col style="width: 60%" />
</colgroup>
<thead>
<tr class="header">
<th>Setting</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td><code>chef_type</code></td>
<td>Always set this to <code>role</code>. Use this setting for any custom process that consumes role objects outside of Ruby.</td>
</tr>
<tr>
<td><code>json_class</code></td>
<td>Always set this to <code>Chef::Role</code>. The Chef Infra Client uses this setting to auto inflate a role object. If objects are being rebuilt outside of Ruby, ignore it.</td>
</tr>
</tbody>
</table>

## Manage Roles

There are several ways to manage roles:

- knife can be used to create, edit, view, list, tag, and delete
    roles.
- The Chef Infra Client can be used to manage role data using the
    command line and JSON files (that contain a hash, the elements of
    which are added as role attributes). In addition, the `run_list`
    setting allows roles and/or recipes to be added to the role.
- The open source Chef Infra Server can be used to manage role data
    using the command line and JSON files (that contain a hash, the
    elements of which are added as role attributes). In addition, the
    `run_list` setting allows roles and/or recipes to be added to the
    role.
- The Chef Infra Server API can be used to create and manage roles
    directly, although using knife directly is the most common way to manage roles.
- The command line can also be used with JSON files and third-party
    services, such as Amazon EC2, where the JSON files can contain
    metadata for each instance stored in a file on-disk and then read by
    chef-solo or Chef Infra Client as required.

By creating and editing files using the Chef Language (Ruby) or JSON, you can dynamically generate role data. Roles created and edited
using files are compatible with all versions of Chef, including
chef-solo. Roles created and edited using files can be kept in version
source control, which also keeps a history of what changed when. When
roles are created and edited using files, they shouldn't be managed
using knife, as changes will be
overwritten.

A run-list that's associated with a role can be edited using the Chef
management console add-on. The canonical source of a role's data is
stored on the Chef Infra Server, which means that keeping role data in
version source control can be challenging.

If roles are created and managed using knife and then arbitrarily updated
uploaded through JSON data, that action will overwrite the previous work with knife.
It's strongly recommended to keep to one process and not switch back and forth.

### Set Run-lists for Environments

Associating a run-list with a role and a specific environment lets you use the run-list on different nodes that share the same environment. More than one environment can be specified in a role, but each specific environment may be associated with only one run-list. If a run-list isn't specified, the default run-list will be used. For example:

```json
{
  "name": "webserver",
  "default_attributes": {
  },
  "json_class": "Chef::Role",
  "env_run_lists": {
    "production": [],
    "preprod": [],
    "test": [ "role[base]", "recipe[apache]", "recipe[apache::copy_test_configs]" ],
    "dev": [ "role[base]", "recipe[apache]", "recipe[apache::copy_dev_configs]" ]
    },
  "run_list": [ "role[base]", "recipe[apache]" ],
  "description": "The webserver role",
  "chef_type": "role",
  "override_attributes": {
  }
}
```

where:

- `webserver` is the name of the role
- `env_run_lists` is a hash of environment run-lists for
    `production`, `preprod`, `test`, and `dev`
- `production` and `preprod` use the default run-list because they do
    not have a shared environment run-list
- `run_list` defines the default run-list

### Delete from Run-list

When an environment is deleted, it will remain within a run-list for a
role until it's removed from that run-list. If a new environment is
created that has an identical name to an environment that was deleted, a
run-list that contains an old environment name will use the new one.
