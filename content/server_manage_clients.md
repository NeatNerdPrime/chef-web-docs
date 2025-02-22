+++
title = "Manage Client Keys"
draft = false
gh_repo = "chef-web-docs"
robots = "noindex"
aliases = ["/server_manage_clients.html"]
product = []

[menu]
  [menu.legacy]
    title = "Clients"
    identifier = "legacy/manage/server_manage_clients.md Clients"
    parent = "legacy/manage"
+++

{{< chef_automate_mark >}}

{{< warning >}}

{{< readfile file="content/reusable/md/EOL_manage.md" >}}

This document is no longer maintained.

{{< /warning >}}

{{< note >}}

This topic is about using the Chef management console to manage keys.

{{< /note >}}

A client is an actor that has permission to access the Chef Infra
Server. A client is most often a node (on which the Chef Infra Client
runs), but is also a workstation (on which knife runs), or some other
machine that's configured to use the Chef Infra Server API. Each
request to the Chef Infra Server that's made by a client uses a private
key for authentication that must be authorized by the public key on the
Chef Infra Server.

Use the Chef management console to create a key pair, download the
private key, and then set permissions, to delete a key, or to reset a
key.

## Manage Client Keys

Client keys can be managed from the Chef management console.

{{< warning >}}

The images below refer to client keys as a "Client".

{{< /warning >}}

### Add

To add a client key:

1. Open the Chef management console.

2. Click **Policy**.

3. Click **Clients**.

4. Click **Create**.

5. In the **Create Client** dialog box, enter the name of the client
    key.

    ![image](/images/step_manage_webui_policy_client_add.png)

    Click **Create Client**.

6. Copy the private key:

    ![image](/images/step_manage_webui_policy_client_add_private_key.png)

    or download and save the private key locally:

    ![image](/images/step_manage_webui_policy_client_add_private_key_download.png)

### Delete

To delete a client key:

1. Open the Chef management console.

2. Click **Policy**.

3. Click **Clients**.

4. Select a client key.

5. Click **Delete**.

    ![image](/images/step_manage_webui_policy_client_delete.png)

### Reset Key

To regenerate a client key:

1. Open the Chef management console.

2. Click **Policy**.

3. Click **Clients**.

4. Select a client key.

5. Click the **Details** tab.

6. Click **Reset Key**.

7. In the **Reset Key** dialog box, confirm that the key should be
    regenerated and click the **Reset Key** button:

    ![image](/images/step_manage_webui_admin_organization_reset_key.png)

8. Copy the private key:

    ![image](/images/step_manage_webui_policy_client_reset_key_copy.png)

    or download and save the private key locally:

    ![image](/images/step_manage_webui_policy_client_reset_key_download.png)

### View Details

To view client key details:

1. Open the Chef management console.
2. Click **Policy**.
3. Click **Clients**.
4. Select a client key.
5. Click the **Details** tab.

### Permissions

{{< readfile file="content/server/reusable/md/server_rbac_permissions.md" >}}

{{< readfile file="content/server/reusable/md/server_rbac_permissions_object.md" >}}

#### Set

To set permissions list for a client key:

1. Open the Chef management console.
2. Click **Policy**.
3. Click **Clients**.
4. Select a client key.
5. Click the **Permissions** tab.
6. For each group listed under **Name**, select or de-select the
    **Read**, **Update**, **Delete**, and **Grant** permissions.

#### Update

{{< readfile file="content/reusable/md/manage_webui_policy_client_permissions_add.md" >}}

#### View

To view permissions for a client key:

1. Open the Chef management console.
2. Click **Policy**.
3. Click **Clients**.
4. Select a client key.
5. Click the **Permissions** tab.
6. Set the appropriate permissions: **Delete**, **Grant**, **Read**,
    and/or **Update**.

## chef-validator Keys

{{< readfile file="content/reusable/md/security_chef_validator.md" >}}

{{< readfile file="content/reusable/md/security_chef_validator_context.md" >}}

### Add

To add a chef-validator key:

1. Open the Chef management console.

2. Click **Policy**.

3. Click **Clients**.

4. Click **Create**.

5. In the **Create Client** dialog box, enter the name of the
    chef-validator key.

    ![image](/images/step_manage_webui_policy_validation_add.png)

    Select the **Validation Client** option. Click **Create Client**.

6. Copy the private key:

    ![image](/images/step_manage_webui_policy_client_add_private_key.png)

    or download and save the private key locally:

    ![image](/images/step_manage_webui_policy_client_add_private_key_download.png)

### Delete

To delete a chef-validator key:

1. Open the Chef management console.

2. Click **Policy**.

3. Click **Clients**.

4. Select a chef-validator key.

5. Click **Delete**.

    ![image](/images/step_manage_webui_policy_validation_delete.png)

### Reset Key

{{< readfile file="content/reusable/md/manage_webui_policy_validation_reset_key.md" >}}

### View Details

To view details for a chef-validator key:

1. Open the Chef management console.

2. Click **Policy**.

3. Click **Clients**.

4. Select a chef-validator key.

    ![image](/images/step_manage_webui_policy_validation_view_details.png)

5. Click the **Details** tab.

### Permissions

{{< readfile file="content/server/reusable/md/server_rbac_permissions.md" >}}

{{< readfile file="content/server/reusable/md/server_rbac_permissions_object.md" >}}

#### Set

To update the permissions list for a chef-validator key:

1. Open the Chef management console.
2. Click **Policy**.
3. Click **Clients**.
4. Select a chef-validator key.
5. Click the **Permissions** tab.
6. Click the **+ Add** button and enter the name of the user or group
    to be added.
7. Select or de-select **Delete**, **Grant**, **Read**, and/or
    **Update** to update the permissions list for the user or group.

#### Update

{{< readfile file="content/reusable/md/manage_webui_policy_client_permissions_add.md" >}}

#### View

To view permissions for a chef-validator key:

1. Open the Chef management console.
2. Click **Policy**.
3. Click **Clients**.
4. Select a chef-validator key.
5. Click the **Permissions** tab.
6. Set the appropriate permissions: **Delete**, **Grant**, **Read**,
    and/or **Update**.
