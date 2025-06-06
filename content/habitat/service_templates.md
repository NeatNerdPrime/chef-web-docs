+++
title = "Service Template Data"

date = 2024-12-27T20:47:49.689Z
draft = false

[menu]
  [menu.habitat]
    title = "Service Template Data"
    identifier = "habitat/reference/service_templates Service Template Data"
    parent = "habitat/reference"
+++

<!-- This is a generated file, do not edit it directly. See https://github.com/habitat-sh/habitat/blob/master/.expeditor/scripts/finish_release/generate-template-reference.js -->

The following settings can be used during a Chef Habitat service's lifecycle. This means that you can use these settings in any of the plan hooks, such as `init`, or `run`, and also in any templatized configuration file for your application or service.

These configuration settings are referenced using the [Handlebars.js](https://github.com/wycats/handlebars.js/) version of [Mustache-style](https://mustache.github.io/mustache.5.html) tags.


## sys

System information

| Property | Type | Description |
| -------- | ---- | ----------- |
| ctl_gateway_ip | string | Listening address for Supervisor's Control Gateway. |
| ctl_gateway_port | integer | Listening port for Supervisor's Control Gateway. |
| gossip_ip | string | Listening address for Supervisor's gossip connection. |
| gossip_port | integer | Listening port for Supervisor's gossip connection. |
| hostname | string | The hostname of the running service. |
| http_gateway_ip | string | Listening address for Supervisor's HTTP gateway. |
| http_gateway_port | integer | Listening port for Supervisor's HTTP gateway. |
| ip | string | The IP address of the running service. |
| member_id | string | The member's Supervisor ID, e.g., `3d1e73ff19464a27aea3cdc5c2243f74` |
| permanent | boolean | Set to true if a Supervisor is being used as a permanent peer, to increase Ring network traffic stability. |
| version | string | Version of the Habitat Supervisor, e.g., `0.54.0/20180221023448` |

## pkg

Details about the package currently running the service

| Property | Type | Description |
| -------- | ---- | ----------- |
| ident | string | The fully-qualified identifier of the running package, e.g., `core/redis/3.2.4/20170514150022` |
| origin | string | The origin of the Habitat package |
| name | string | The name of the Habitat package |
| version | string | The version of the Habitat package |
| release | string | The release of the Habitat package |
| deps | array | An array of runtime dependencies for your package based on the `pkg_deps` setting in a plan |
| env | object | The runtime environment of your package, mirroring the contents of the `RUNTIME_ENVIRONMENT` metadata file. The `PATH` variable is set, containing all dependencies of your package, as well as any other runtime environment variables that have been set by the package. Individual variables can be accessed directly, like `{{pkg.env.PATH}}` (the keys are case sensitive). |
| exposes | array | The array of ports to expose for an application or service. This value is pulled from the pkg_exposes setting in a plan. |
| exports | object | A map of export key to internal configuration value key (i.e., the contents of `pkg_exports` in your plan). The key is what external services consume. The value is a key in your `default.toml` file that corresponds to the data being exported. |
| path | string | The location where the package is installed locally, e.g., `/hab/pkgs/core/redis/3.2.4/20170514150022`. Note that this is _not_ a `PATH` environment variable; for that, please see the `env` key above. |
| svc_path | string | The root location of the source files for the Habitat service, e.g., `/hab/svc/redis`. |
| svc_config_path | string | The location of any templated configuration files for the Habitat service, e.g., `/hab/svc/redis/config`. |
| svc_data_path | string | The location of any data files for the Habitat service, e.g., `/hab/svc/redis/data`. |
| svc_files_path | string | The location of any gossiped configuration files for the Habitat service, e.g., `/hab/svc/redis/files`. |
| svc_static_path | string | The location of any static content for the Habitat service, e.g., `/hab/svc/redis/static`. |
| svc_var_path | string | The location of any variable state data for the Habitat service, e.g., `/hab/svc/redis/var`. |
| svc_pid_file | string | The location of the Habitat service pid file, e.g., `/hab/svc/redis/PID`. |
| svc_run | string | The location of the rendered run hook for the Habitat service, e.g., `/hab/svc/redis/run`. |
| svc_user | string | The value of `pkg_svc_user` specified in a plan. |
| svc_group | string | The value of `pkg_svc_group` specified in a plan. |

## cfg

These are settings defined in your templatized configuration file. The values for those settings are pulled from the `default.toml` file included in your package.

## svc

Information about the current service's service group

| Property | Type | Description |
| -------- | ---- | ----------- |
| service | string | The name of the service. If the service is running from the package `core/redis`, the value will be `redis`. |
| group | string | The group portion of the service's complete group name. In the group name `redis.default`, the group's value is `default`. |
| org | string | The organization portion of a service group specification. Unused at this time. |
| election_is_running | boolean | Whether a leader election is currently running for this service |
| election_is_no_quorum | boolean | Whether there is quorum for a leader election for this service |
| election_is_finished | boolean | Whether a leader election for this service has finished |
| update_election_is_running | boolean | Whether an update leader election is currently running for this service |
| update_election_is_no_quorum | boolean | Whether there is quorum for an update leader election for this service |
| update_election_is_finished | boolean | Whether an update leader election for this service has finished |
| me | [svc_member](#svc_member) | An object that provides information about the service running on the local Supervisor |
| first | [svc_member](#svc_member) | The first member of this service group, or the leader, if running in a leader topology |
| members | array | All active members (`alive` and `suspect`) of the service group, across the entire ring. As of 0.56.0, does _not_ include `departed` or `confirmed` members |
| leader | [svc_member](#svc_member) | The current leader of the service group, if any (`null` otherwise) |
| update_leader | [svc_member](#svc_member) | The current update_leader of the service group, if any (`null` otherwise) |

## bind

Exposes information about the service groups this service is bound to. Each key is the name of a bind, while each value is one of the objects described below

| Property | Type | Description |
| -------- | ---- | ----------- |
| first | [svc_member](#svc_member) | The first member of this service group. If the group is running in a leader topology, this will also be the leader. |
| leader | [svc_member](#svc_member) | The current leader of this service group, if running in a leader topology |
| members | array | All active members (`alive` and `suspect`) of the service group, across the entire ring. As of 0.56.0, does _not_ include `departed` or `confirmed` members |

## Reference Objects

Some of the template expressions referenced above return objects of a specific shape; for example, the `svc.me` and `svc.first` expressions return "service member" objects, and the `pkg` property of a service member returns a "package identifier" object. These are defined below.

## package_identifier

A Habitat package identifier, split apart into its constituent components

| Property | Type | Description |
| -------- | ---- | ----------- |
| origin | string | The origin of the Habitat package |
| name | string | The name of the Habitat package |
| version | string | The version of the Habitat package |
| release | string | The release of the Habitat package |

## svc_member

Represents a member of a service group

| Property | Type | Description |
| -------- | ---- | ----------- |
| member_id | string | the member's Supervisor id, e.g., 3d1e73ff19464a27aea3cdc5c2243f74 |
| alive | boolean | Whether this member is considered alive and connected to the ring, from a network perspective. |
| suspect | boolean | Whether this member is considered "suspect", or possibly unreachable, from a network perspective. |
| confirmed | boolean | Whether this member is confirmed dead / unreachable, from a network perspective. |
| departed | boolean | Whether this member has been departed from the ring (i.e., permanently gone, never to return). |
| election_is_running | boolean | Whether a leader election is currently running for this service |
| election_is_no_quorum | boolean | Whether there is quorum for a leader election for this service |
| election_is_finished | boolean | Whether a leader election for this service has finished |
| update_election_is_running | boolean | Whether an update leader election is currently running for this service |
| update_election_is_no_quorum | boolean | Whether there is quorum for an update leader election for this service |
| update_election_is_finished | boolean | Whether an update leader election for this service has finished |
| leader | boolean | Whether this member is the leader in the service group (only meaningful in a leader topology) |
| follower | boolean | Whether this member is a follower in the service group (only meaningful in a leader topology) |
| update_leader | boolean | Whether this member is the update leader in the service group (only meaningful in a leader topology) |
| update_follower | boolean | Whether this member is an update follower in the service group (only meaningful in a leader topology) |
| pkg | [package_identifier](#package_identifier) | The identifier of the release the member is running |
| pkg_incarnation | integer | Incarnation number associated with this package update |
| package | string | The package identifier |
| sys | object | An abbreviated version of the top-level {{sys}} object, containing networking information for the member. |
| cfg | object | The configuration the member is currently exporting. This is constrained by what is defined in `pkg_exports`, where the values are replaced with the current values (e.g., taking into account things like user.toml, gossiped configuration values, etc.) |
| persistent | boolean | A misspelling of `permanent`; indicates whether a member is a permanent peer or not |
| service | string | The name of the service. If the service is running from the package `core/redis`, the value will be `redis`. |
| group | string | The group portion of the service's complete group name. In the group name `redis.default`, the group's value is `default`. |
| org | string | The organization portion of a service group specification. Unused at this time. |
