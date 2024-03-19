# Using System Objects with Custom Objects

## [Accessing Accounts Data from Custom Objects](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/using-system-objects-with-custom-objects/accessing-accounts-data-from-custom-objects#accessing-accounts-data-from-custom-objects) <a href="#accessing-accounts-data-from-custom-objects" id="accessing-accounts-data-from-custom-objects"></a>

Liferay 7.4 U35+/GA35+

In some use cases, you may want to access account data from a custom object. For example, if you’re building a custom returns application, you may want to prompt users to select their desired account and postal address when creating a return request. In this scenario, users would need access to their existing account and address entries while in the returns application.

To achieve this, Liferay provides access to the Account and Postal Address applications in the Objects admin. Here, you can define relationships between them and your custom objects. Once defined, users can use the generated relationship fields in the custom object to select their desired Account and Postal Address entries.

![Create relationships from the Account and Postal Address objects to your custom object.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/using-system-objects-with-custom-objects/accessing-accounts-data-from-custom-objects/images/01.png)

NOTE

The Account object stores general account data (e.g., name, email), while the Postal Address object stores billing and shipping addresses for an account. Postal Address entries are created in the Accounts UI and are scoped to the individual account for which they’re created.

When relating a custom object to the Accounts and Postal Address system objects, these relationships should be defined on the system object side and use the one-to-many type. The custom object should be on the many side of both relationships.

Follow these steps to set up relationships for accessing Account and Postal Address entries from your custom object:

1. Open the _Global Menu_ (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)), go to the _Control Panel_ tab, and click _Objects_.
2. Begin editing the _Accounts_ object.
3. Go to the _Relationships_ tab and click the _Add_ button (![Add Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png)).
4. Enter a _Label_ and _Name_.
5. For Type, select _One to Many_.
6.  For Object, select your _custom object_.

    ![Create a one-to-many relationship from the Account object to your custom object.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/using-system-objects-with-custom-objects/accessing-accounts-data-from-custom-objects/images/02.png)
7. Click _Save_.
8. Return to the Objects admin page and begin editing the _Postal Address_ object.
9. Go to the _Relationships_ tab and click _Add_ (![Add Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png)).
10. Enter a _Label_ and _Name_.
11. For Type, select _One to Many_.
12. For Object, select the same _custom object_ as you did for the previous relationship.
13. For Parameter, select the custom object’s _relationship with the Account object_ that you created earlier.

    This is necessary because Postal Address entries are scoped to specific accounts. The parameter sets up a dependent relationship where available addresses depend on the Account relationship’s field value.

    ![Create a one-to-many relationship from the Postal Address object to your custom object and use the custom object's Account relationship as a parameter.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/using-system-objects-with-custom-objects/accessing-accounts-data-from-custom-objects/images/03.png)
14. Click _Save_.
15. Add the relationship fields to the custom object’s default layout and view. See [Designing Object Layouts](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/layouts/designing-object-layouts) and [Designing Object Views](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/views/designing-object-views) for detailed instructions.

    ![Add the relationship fields to the custom object's default layout and view.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/using-system-objects-with-custom-objects/accessing-accounts-data-from-custom-objects/images/04.png)

Once added, users can select from available accounts and addresses in the custom object. Available account options depend on the user’s account memberships, and available addresses depend on the selected account.

![Users can select from available accounts and addresses in the custom object.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/using-system-objects-with-custom-objects/accessing-accounts-data-from-custom-objects/images/05.png)

TIP

By default, objects use entry IDs for their title fields. The title field value determines how each entry option appears to end users in the relationship field. If desired, you can improve readability by configuring each object to use a different field for the title.

To do this, begin editing the desired object definition, go to the _Details_ tab, and select a different value for the _Title Field_. Click _Save_ when finished.

## Restricting Access to Object Data by Account

Liferay 7.4 U58+/GA58+

In scenarios with multiple business [accounts](https://learn.liferay.com/web/guest/w/dxp/users-and-permissions/accounts), you may want to restrict access to object entries by account. For example, consider an [insurance scenario](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/using-system-objects-with-custom-objects/restricting-access-to-object-data-by-account#example-use-case-insurance-claims) with these requirements:

* Business account users must open claims without seeing claims made by other accounts.
* Customer success managers (CSMs) must review claims made by the accounts they manage.
* A claims manager must oversee all claims.

To restrict object entries by account, your custom object must have an active [relationship](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/relationships/defining-object-relationships) with the Account system object, and you must configure the custom object to use this relationship for restricting entry data. Once set up, anyone adding entries to the custom object must select an account. This account becomes the entry’s owner. See [Account Restriction and User Roles](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/using-system-objects-with-custom-objects/restricting-access-to-object-data-by-account#account-restriction-and-user-roles) to learn how role types control access to entries.

![Restrict access to custom object entries by account.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/using-system-objects-with-custom-objects/restricting-access-to-object-data-by-account/images/01.png)

IMPORTANT

* You can only configure account restriction for draft objects. After [publishing](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/creating-objects#publishing-object-drafts), these settings cannot be changed.
* For Liferay DXP 2024.Q1+/Liferay Portal 7.4 GA112+, the entries of account restricted objects appear in search results if the search user has the `Accounts > Account Entry: View` permission in an account- or regular-scoped role. See [Searching for Content](https://learn.liferay.com/web/guest/w/dxp/using-search/getting-started/searching-for-content#searching-for-liferay-objects) for details.

To set up account restriction,

1. Open the _Global Menu_ (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)), go to the _Control Panel_ tab, and click _Objects_.
2. Begin editing the Account system object.
3.  Add a One to Many relationship that relates the Account object to the desired draft object. See [Defining Object Relationships](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/relationships/defining-object-relationships) for more information.

    ![Relate the Account system object to the desired custom object.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/using-system-objects-with-custom-objects/restricting-access-to-object-data-by-account/images/02.png)
4. Return to the Objects page and begin editing the desired custom object.
5.  In the Details tab, activate _Account Restriction_ and select the account relationship to use.

    ![Toggle Account Restriction to active and select the account relationship you want to use.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/using-system-objects-with-custom-objects/restricting-access-to-object-data-by-account/images/03.png)
6. Click _Save_.

This makes the account relationship field mandatory, so users must select an account when creating entries. Available account options are determined by each user’s permissions. See [Account Restriction and User Roles](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/using-system-objects-with-custom-objects/restricting-access-to-object-data-by-account#account-restriction-and-user-roles) for guidance on configuring role permissions while account restriction is enabled. After entry creation, the selected account becomes the entry’s owner and cannot be changed.

### [Account Restriction and User Roles](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/using-system-objects-with-custom-objects/restricting-access-to-object-data-by-account#account-restriction-and-user-roles) <a href="#account-restriction-and-user-roles" id="account-restriction-and-user-roles"></a>

After restricting object data by account, entries appear according to the user’s permissions. Permissions at the regular role scope apply to all object data, while permissions at the organization or account scope apply to specific accounts. For example, if users have a regular role with the view permission, they can view all data from all accounts. However, if users only have an account role with the view permission, they can only view object data associated with their accounts.

| Permission Scope  | Description                                                                                                       |
| ----------------- | ----------------------------------------------------------------------------------------------------------------- |
| Regular Role      | Permissions apply to all object data without restriction by account or organization role permissions.             |
| Organization Role | Permissions apply to object data for specific accounts associated with an organization and its sub-organizations. |
| Account Role      | Permissions apply to object data for a specific account.                                                          |

When setting up roles for account restriction, consider these factors:

* Account restricted objects must have _Show Widget_ enabled and must set a [panel link](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/creating-objects) to appear in the Roles UI.
* Regular roles must have the `Accounts > Account Entry: View` permission to create object entries. This is because regular roles are not associated with accounts or organizations. Without this permission, users with the regular role cannot select an account during entry creation.
* Organization roles must have these permissions to create object entries: `Users and Organizations &rarr; Organization: Manage Accounts` and `Users and Organizations &rarr; Organization: Manage Suborganizations Accounts`. Without these permissions, users with the organization role cannot select an account during entry creation.
* For organization and account role users to access a custom object, they must have a separate regular role granting the `Access in Control Panel` (company-scoped) or `Access in Site` permission (site-scoped). See [Object Application Permissions](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/understanding-object-integrations/permissions-framework-integration#application-permissions) for more information.

IMPORTANT

Account and organization roles are only supported in objects with account restriction enabled.

Account restriction does not support the default Account Member and Organization user roles. These roles do not grant access automatically to related object data.

When a user is a member of multiple accounts with different account roles and object permissions, these permissions apply to each account the user belongs to.

### [Example Use Case: Insurance Claims](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/using-system-objects-with-custom-objects/restricting-access-to-object-data-by-account#example-use-case-insurance-claims) <a href="#example-use-case-insurance-claims" id="example-use-case-insurance-claims"></a>

Consider an insurance scenario with these requirements:

* Business account users must open claims without seeing claims made by other accounts.
* Customer success managers (CSMs) must review claims made by the accounts they manage.
* A claims manager must oversee all claims.

To achieve this, create a Claims object with account restriction enabled. Then create organizations for grouping CSMs and associate each organization with the appropriate business accounts. Finally, create the following [account](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/using-system-objects-with-custom-objects/restricting-access-to-object-data-by-account#account-role), [organization](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/using-system-objects-with-custom-objects/restricting-access-to-object-data-by-account#organization-role), and [regular](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/using-system-objects-with-custom-objects/restricting-access-to-object-data-by-account#regular-role) roles and assign them to the appropriate users. Since the Claims object is company-scoped, account and organization users must also have a separate regular role that grants the `Portal: View Control Panel Menu` and `Claims: Access in Control Panel` permissions. For convenience, you can assign these permissions to the default user role.

NOTE

In this example, each role can create claim entries, but granting that permission is not necessary for the organization and regular roles.

#### [Account Role](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/using-system-objects-with-custom-objects/restricting-access-to-object-data-by-account#account-role) <a href="#account-role" id="account-role"></a>

Grant account users permission to create and manage the account’s object data.

| Permission                        | Description                                                |
| --------------------------------- | ---------------------------------------------------------- |
| Claims: View                      | View the object’s application page.                        |
| Claims > Claim: Delete            | Delete object entries.                                     |
| Claims > Claim: Permissions       | View and modify permissions for individual object entries. |
| Claims > Claim: Update            | Update object entries.                                     |
| Claims > Claims: Add Object Entry | Create an object entry.                                    |

IMPORTANT

Do not assign the `Claims &rarr; Claim: View` resource permission to account roles. This permission can interfere with standard role behaviors.

#### [Organization Role](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/using-system-objects-with-custom-objects/restricting-access-to-object-data-by-account#organization-role) <a href="#organization-role" id="organization-role"></a>

Grant organization users permission to create and manage object data for all accounts associated with the organization and its sub-organizations.

| Permission                                                              | Description                                                |
| ----------------------------------------------------------------------- | ---------------------------------------------------------- |
| Claims: View                                                            | View the object’s application page.                        |
| Claims > Claim: Delete                                                  | Delete object entries.                                     |
| Claims > Claim: Permissions                                             | View and modify permissions for individual object entries. |
| Claims > Claim: Update                                                  | Update object entries.                                     |
| Claims > Claim: View                                                    | View object entries.                                       |
| Claims > Claims: Add Object Entry                                       | Create an object entry.                                    |
| User and Organizations > Organization: Manage Accounts                  | View an organization’s accounts.                           |
| User and Organizations > Organization: Manage Suborganizations Accounts | View accounts associated with sub-organizations.           |

#### [Regular Role](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/using-system-objects-with-custom-objects/restricting-access-to-object-data-by-account#regular-role) <a href="#regular-role" id="regular-role"></a>

Grant users permission to create and manage object data for all accounts.

| Permission                        | Description                                                                                                                               |
| --------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| Accounts > Account Entry: View    | View access to account entries.                                                                                                           |
| Portal: View Control Panel Menu   | Access to the Global Menu ( ![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png) ). |
| Claims: Access in Control Panel   | Access the object in the Global Menu.                                                                                                     |
| Claims: View                      | View the object’s application page.                                                                                                       |
| Claims > Claim: Delete            | Delete object entries.                                                                                                                    |
| Claims > Claim: Permissions       | View and modify permissions for individual object entries.                                                                                |
| Claims > Claim: Update            | Update object entries.                                                                                                                    |
| Claims > Claim: View              | View object entries.                                                                                                                      |
| Claims > Claims: Add Object Entry | Create an object entry.                                                                                                                   |
