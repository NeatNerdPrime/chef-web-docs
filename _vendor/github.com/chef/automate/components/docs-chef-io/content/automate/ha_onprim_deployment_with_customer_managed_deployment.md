+++
title = "On-Prem Deployment with Customer Managed Database"
draft = false
gh_repo = "automate"

[menu]
  [menu.automate]
    title = "On-Prem Deployment with Customer Managed Database"
    parent = "automate/deploy_high_availability/deployment"
    identifier = "automate/deploy_high_availability/deployment/ha_onprim_deployment_with_customer_managed_deployment.md On-premise Deployment with Customer Managed Database"
    weight = 220
+++

{{< note >}}
{{% automate/ha-warn %}}
{{< /note >}}

This section will discuss deploying Chef Automate HA on-premises machines with a customer-managed database. Please see the [On-Premises Prerequisites](/automate/ha_on_premises_deployment_prerequisites/) page and move ahead with the following sections of this page.

{{< warning >}}

- If SELinux is enabled, deployment with configure it to `permissive` (Usually in case of RHEL SELinux is enabled)
- The SSH user should have execute permissions on the `/tmp` directory.

{{< /warning >}}

- Before proceeding with deployment steps make sure to [provision](/automate/ha_onprim_deployment_procedure/#provisioning).

- [Run on the Bastion host](/automate/ha_onprim_deployment_procedure/#deploy-the-bastion-host) to download the latest Automate CLI and Airgapped Bundle.

## Generate Chef Automate config

1. [Generate](/automate/ha_config_gen) the configuration file.

    ```bash
    sudo chef-automate config gen config.toml
    ```

    You can also view the [Sample Config](#sample-config-to-setup-on-premises-deployment-with-self-managed-services).

    {{< note spaces=4 >}}

    You can also generate config using the `init-config-ha` subcommand and to generate init config for existing infrastructure.

    `chef-automate init-config-ha existing_infra`

    {{< /note >}}

## Verify

### Prerequisites

#### * Directory Structure

- The verification cli needs `$HOME` environment variable to be available on all nodes. 
- If in some case its not available then as a fallback the cli will be copied over to `/home/<ssh_user name>/`.
  - `ssh_user name` is read from `ssh_user` property in `config.toml`
- Every node must have the `$HOME` directory with minimum permissions `drwx------`.

#### * Permission Requirements

- The specified SSH user must have:
  - Read (r), write (w), and execute (x) permissions.
  - Ownership of the directory.

Verify the configuration file.

```bash
sudo chef-automate verify -c config.toml
```

To learn more about Config Verify, check the [Config Verify Doc page](/automate/ha_verification_check/).

Once the verification is completed, proceed with deployment. In case of failure, fix the issue and re-run the verify command.

## Steps to Deploy

The following command will run the deployment.

```bash
chef-automate deploy config.toml --airgap-bundle automate.aib
```

To skip verification in the deploy command, use `--skip-verify` flag

```bash
chef-automate deploy config.toml --airgap-bundle automate.aib --skip-verify
```

## Verify Deployment

1. Once the deployment is successful, get the consolidate status of the cluster.

    ```bash
     chef-automate status summary
    ```

1. Get the service status from each node.

    ```bash
     chef-automate status
    ```

1. Run the verification command.

    ```bash
     chef-automate verify
    ```

1. Get the cluster information.

    ```bash
     chef-automate info
    ```

Check if Chef Automate UI is accessible by going to (Domain used for Chef Automate) [https://chefautomate.example.com](https://chefautomate.example.com).

After successful deployment, proceed with the following:

   1. [Create users and organizations](/automate/ha_node_bootstraping/#create-users-and-organization)
   1. [Workstation setup](/automate/ha_node_bootstraping/#workstation-setup)
   1. [Node bootstrapping](/automate/ha_node_bootstraping/#bootstraping-a-node)

## Backup/Restore

A shared file system is always required to create OpenSearch snapshots. To register the snapshot repository using OpenSearch, it is necessary to mount the same shared filesystem to the exact location on all master and data nodes. To know more about the backup and restore configuration, see On-Premise Deployment using [Filesystem](/automate/ha_backup_restore_file_system) or using [Object Storage](/automate/ha_backup_restore_object_storage).

## Add/Remove Nodes

The Chef Automate commands require some arguments so that it can determine which types of nodes you want to add or remove to/from your HA setup from your bastion host. To know more see [Add Nodes to the Deployment](/automate/ha_add_nodes_to_the_deployment) to add nodes and [Remove Single Node from Cluster](/automate/ha_remove_single_node_from_cluster) to remove nodes.

## Patch Configs

The bastion server can patch new configurations in all nodes. To know more see [Patch Configuration](/automate/ha_config/#patch-configuration) section.

## sample config to set up on-premises deployment with self managed services

```toml
[architecture]
  [architecture.existing_infra]
    ssh_user = "ec2-user"
    ssh_group_name = "ec2-user"
    ssh_key_file = "/home/ec2-user/KEY_FILENAME.pem"
    ssh_port = "22"
    secrets_key_file = "/hab/a2_deploy_workspace/secrets.key"
    secrets_store_file = "/hab/a2_deploy_workspace/secrets.json"
    architecture = "existing_nodes"
    workspace_path = "/hab/a2_deploy_workspace"
    backup_mount = "/mnt/automate_backups"
    backup_config = "object_storage"
[object_storage]
  [object_storage.config]
    bucket_name = "example-bucket"
    access_key = "JVS......."
    secret_key = "VIK........"
    endpoint = "https://objectstorage.example.com"
[automate]
  [automate.config]
    admin_password = "adminpassword"
    fqdn = "chefautomate.example.com"
    config_file = "configs/automate.toml"
    root_ca = "-----BEGIN CERTIFICATE-----
    -----END CERTIFICATE-----"
    instance_count = "2"
[chef_server]
  [chef_server.config]
    fqdn = "chefinfraserver.example.com"
    lb_root_ca = "-----BEGIN CERTIFICATE-----
    -----END CERTIFICATE-----"
    instance_count = "2"
[opensearch]
  [opensearch.config]
    instance_count = "0"
[postgresql]
  [postgresql.config]
    instance_count = "0"
[existing_infra]
  [existing_infra.config]
    automate_private_ips = ["192.0.0.1", "192.0.0.2"]
    chef_server_private_ips = ["192.0.0.3", "192.0.0.4"]
[external]
  [external.database]
    type = "self-managed"
    [external.database.postgre_sql]
      instance_url = "pg.example.com:5432"
      superuser_username = "superusername"
      superuser_password = "superuserpassowrd"
      dbuser_username = "databaseusername"
      dbuser_password = "databaseuserpassword"
      postgresql_root_cert = "-----BEGIN CERTIFICATE-----
      -----END CERTIFICATE-----"
    [external.database.open_search]
      opensearch_domain_name = "opensearch-domain"
      opensearch_domain_url = "opensearch.example.com:9200"
      opensearch_username = "opensearchusername"
      opensearch_user_password = "opensearchuserpassword;"
      opensearch_root_cert = "-----BEGIN CERTIFICATE-----
      -----END CERTIFICATE-----"
```

## Uninstall Chef Automate HA

To uninstall Chef Automate HA instances after unsuccessful deployment, run the below command in your bastion host.

```bash
chef-automate cleanup --onprem-deployment
```
