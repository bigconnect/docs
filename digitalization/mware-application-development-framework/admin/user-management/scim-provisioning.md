# SCIM provisioning

SCIM (System for Cross-domain Identity Management) is an open standard for managing user identity information.

It allows you to manage your users and groups outside of the system using any Identity and Access Management service that supports SCIM. Examples include _Azure Active Directory_ and _Okta_.

Changes to users and groups made in your external IAM can be synced across to the system users and groups through a process known as **provisioning**.

It is worth noting that this is a one way process. When SCIM is enabled, you will no longer be able to edit users and groups; it will only be possible to make changes via your external service.

### Enable SCIM

Login to the portal, and click on the `Settings` tab. Select the `Auth` tab.

<figure><img src="https://files.readme.io/44a9a9db5f559e8869f52173a040090efbe546573559ca79d382ad11568e6817-Screenshot_2024-09-09_at_13.55.16.png" alt=""><figcaption></figcaption></figure>

Scroll to the bottom of the page, and under SCIM toggle _Activated_ on. The provisioning _URL_ and _Token_ will become available to quickly copy using the clipboard buttons to the right of the fields.

<figure><img src="https://files.readme.io/1242196-settings_auth_scim.png" alt=""><figcaption></figcaption></figure>

> ### 👍Enforced SSO
>
> When using SCIM, users can only log-in using [SSO](https://docs.budibase.com/docs/authentication-and-sso). With this in mind, we recommend that you Enforce SSO

***

\


### Example: Azure Active Directory

This example will look at provisioning active directory users from Azure\


#### Step 1 - Create an enterprise application

First we need to create an **Enterprise application** to manage the users and groups that we want to provision.

Log-in to Azure Active Directory, and click `Enterprise applications` under the _Manage_ section.

![](https://files.readme.io/357bea8-manage\_enterprise\_app.png)

<figure><img src="https://files.readme.io/8e893a4-new_enterprise_app.png" alt="Create new application"><figcaption><p>Create new application</p></figcaption></figure>

Click on `New application`, then click on `Create your own application`. Select the _(Non-gallery)_ option and give your app a name.

![](https://files.readme.io/51453ab-create\_your\_own\_app.png)

Assuming you already have some users in your active directory, you can now add them to your enterprise application.

Click on your application, and then click `Users and groups` under the _Manage_ section. You can then click on `Add user/group` to add users individually or user groups.

\


<figure><img src="https://files.readme.io/ef06744-ent_app_users.png" alt=""><figcaption></figcaption></figure>

#### Step 2 - Provisioning

Under the _Manage_ section click on `Provisioning`. Next select 'Automatic' under _Provisioning Mode_. Copy and paste the _URL_ and _Token_ from the settings page as the admin credentials.

<figure><img src="https://files.readme.io/55eba2c-automatic_provisioning.png" alt=""><figcaption></figcaption></figure>

Click `Test Connection` to verify everything is correct, and make sure to click the `Save` button at the top.

You can now click `Start provisioning` to sync your users and groups from your Azure enterprise app.

![](https://files.readme.io/d3a3f27-start\_provisioning.png)

Under the `Users` tab in the portal, you can now see the users that have synced across.

<figure><img src="https://files.readme.io/63e45d2-budibase_synced_users.png" alt=""><figcaption></figcaption></figure>

There is also a note in this section indicating that users are being synced from your AD.

If you provide a first and last name for your Azure users, this will also be passed through, however other settings such as User roles and App roles must be set.

![](https://files.readme.io/45086bb-first\_and\_lastnames.png)![](https://files.readme.io/2ce8015-anna\_payne\_budibase.png)\


#### Step 3 - Setup SSO and give app access

Finally we need to make sure that provisioned users can log-in to the system. In this case you can follow the SSO with Azure AD guide.

Furthermore make sure you have given your users and groups Application access.

<figure><img src="https://files.readme.io/b449fc0-carol_app_access.png" alt="Giving Carol basic app access" width="640"><figcaption><p>Giving Carol basic app access</p></figcaption></figure>

### Managing SCIM

#### Initial SCIM Sync Activation

**What happens when a SCIM sync is activated for the first time?**

* User Matching
  * Existing users are matched with SCIM users by email.
  * Matched users are marked as externally synced and managed externally thereafter.
* Group Matching:
  * Existing groups are matched by name.
  * Groups with matching names (e.g., “engineering”) are taken over as externally synced.
* User Assignment in Synced Groups:
  * Existing assigned users will be wiped out
  * SCIM specified users will be mapped to the group.

#### Managing SCIM Users

**How to manage SCIM users?**

* Identifying Synced Users:
  * Synced users are marked with a sync logo in the builder.
  * Certain fields will be read-only for synced users.
* Fields for Synced Users:
  * Read-Only Fields: Email, name, and surname (managed via external SCIM datasource).
  * Editable Fields: Role and other non-synced fields can be edited.
* Non-SCIM Users:
  * Users created manually or not matched with SCIM are fully editable.

#### Managing SCIM Groups

#### How to manage SCIM groups?

* Identifying Synced Groups:
  * Synced groups are marked with a sync logo in the builder.
  * Some fields will be read-only for synced groups.
* Editable Fields for Synced Groups:
  * Read-Only Fields: Group name (managed via SCIM).
  * Editable Fields: Icons and other non-synced fields can be edited .
* Non-SCIM Groups:
  * Groups created manually or not matched with SCIM are fully managed.
