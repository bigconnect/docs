# Organization Roles

Liferay [Organizations](https://learn.liferay.com/web/guest/w/dxp/users-and-permissions/organizations/understanding-organizations) provide a convenient way to implement distributed User management. With them, you can use Organization-scoped Roles to assign standard permissions to all members of an Organization, as well as assign more specialized permissions to Users based on their specific responsibilities. Doing this can save you time and help ensure that your portal design corresponds to your real-world hierarchy.

### [Understanding Organization Roles](https://learn.liferay.com/web/guest/w/dxp/users-and-permissions/organizations/organization-roles#understanding-organization-roles) <a href="#understanding-organization-roles" id="understanding-organization-roles"></a>

Organization role permissions are defined at the global level and are scoped to the specific Organization in which they’re assigned. These roles also grant Users corresponding, implicit privileges in a connected [Organization Site](https://learn.liferay.com/web/guest/w/dxp/users-and-permissions/organizations/organization-sites). However, some Organization roles also grant privileges in child Organizations and their Sites. If desired, you can use Site Roles and create [Site Teams](https://learn.liferay.com/web/guest/w/dxp/site-building/sites/site-membership/creating-teams-for-sites) to explicitly assign additional roles to Users within an Organization Site. See [Understanding Roles and Permissions](https://learn.liferay.com/web/guest/w/dxp/users-and-permissions/roles-and-permissions/understanding-roles-and-permissions) for more information.

NOTE

By default, Organization roles do not grant access to the Control Panel. However, Organization Administrators and Owners can access any Organizations they manage by clicking on _My Organizations_ in the dropdown _Personal Menu_.

Liferay DXP provides the following default Organization Roles:

#### [Organization User](https://learn.liferay.com/web/guest/w/dxp/users-and-permissions/organizations/organization-roles#organization-user) <a href="#organization-user" id="organization-user"></a>

This role grants basic privileges within an Organization and is automatically assigned to all its members. If the Organization has an attached Site, this role also grants the User basic Site membership.

NOTE

Members of child Organizations are implicit members of their parent Organizations. This means, for example, that members of child Organizations can access the pages of their parent Organizations. This behavior can be customized in the `Organizations` [section of the portal-ext.properties](https://learn.liferay.com/reference/latest/en/dxp/propertiesdoc/portal.properties.html#Organizations) file where the properties specific to Organizations are listed.

#### [Organization Administrator](https://learn.liferay.com/web/guest/w/dxp/users-and-permissions/organizations/organization-roles#organization-administrator) <a href="#organization-administrator" id="organization-administrator"></a>

This role grants super-User privileges within an Organization and its Site, as well as within child Organizations and their Sites. This includes the ability to create and remove child Organizations, assign existing Organization members to a child Organization, create and remove Organization Sites, and create new portal Users within Organization affiliations. Within Organization Sites, this role grants access to the Site menu, as well as Content, Accounts, Search Tuning, Communication, App Builder, and Custom Apps in the Applications tab of the Global Menu. However, this role does not grant the ability to assign or remove Organization Administrators or Organization Owners.

#### [Organization Owner](https://learn.liferay.com/web/guest/w/dxp/users-and-permissions/organizations/organization-roles#organization-owner) <a href="#organization-owner" id="organization-owner"></a>

This role grants all privileges granted by the Organization Administrator role along with the ability to assign or remove Organization Administrators and Organization Owners. However, the Organization Owner privileges remain scoped to the Organization in which they are given, as well as any child Organizations or attached Sites.

### [Assigning Organization Roles](https://learn.liferay.com/web/guest/w/dxp/users-and-permissions/organizations/organization-roles#assigning-organization-roles) <a href="#assigning-organization-roles" id="assigning-organization-roles"></a>

Follow these steps to assign an Organization role to existing Organization Users:

1. Open the _Global Menu_, and go to _Control Panel_ → _Users_ → _Users and Organizations_. Then, click on the _Organizations_ tab.
2.  Click on the _Actions_ button (![Actions Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) for an existing Organization, and select _Assign Organization Roles_.

    ![Select Assign Organization Roles.](https://resources.learn.liferay.com/images/dxp/latest/en/users-and-permissions/organizations/organization-roles/images/01.png)
3.  Click on the _Organization role_ you want to assign to Users.

    ![Click on the Organization role you want to assign to Users](https://resources.learn.liferay.com/images/dxp/latest/en/users-and-permissions/organizations/organization-roles/images/02.png)
4.  Use the _checkboxes_ to select which Users are assigned the role.

    ![Use the checkboxes to select which Users are assigned the role.](https://resources.learn.liferay.com/images/dxp/latest/en/users-and-permissions/organizations/organization-roles/images/03.png)
5. Click on _Update Associations_ when finished.

\
