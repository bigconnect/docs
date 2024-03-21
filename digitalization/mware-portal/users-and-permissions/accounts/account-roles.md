# Account Roles

You can create different Account Roles and define specific permissions for each one. Then assign these Roles to different Account Users to grant appropriate access to their Accounts.

Note that account roles are assigned to a user for each specific account. This offers great flexibility in managing users. For example, a user might have a “Buyer” role for account “A” but not have that role for account “B”. To learn more more about permissions and how roles inherit permissions, see Defining Role Permissions.

### View Account Roles <a href="#view-account-roles" id="view-account-roles"></a>

There are two types of Account Roles: Shared and Owned. Shared type Account Roles are available across all Accounts in a portal Instance. Owned type Account Roles are created and managed within a specific Account.

#### View Shared Account Roles <a href="#view-shared-account-roles" id="view-shared-account-roles"></a>

1. Open the Global Menu (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)). Navigate to _Control Panel_ → _Roles_.
2. Click the _Account Roles_ tab and view the list of available Roles.

#### View Owned Account Roles <a href="#view-owned-account-roles" id="view-owned-account-roles"></a>

1. Open the Global Menu (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)). Navigate to _Control Panel_ → _Accounts_.
2. Click on the specific Account you wish to view. Click the _Roles_ tab and view the list of available Roles.

### Creating a New Account Role <a href="#creating-a-new-account-role" id="creating-a-new-account-role"></a>

Make a choice to create an Shared Account Role or an Owned Account Role.

#### Creating and Assigning a Shared Account Role <a href="#creating-and-assigning-a-shared-account-role" id="creating-and-assigning-a-shared-account-role"></a>

1. Open the Global Menu (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)). Navigate to _Control Panel_ → _Roles_.
2. Click the _Account Roles_ tab. Click the _Add_ icon (![Add icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png)).
3.  Give the new Role a title and description. Click _Save_ to create the Role.

    ![Give the role a new title and description.](https://resources.learn.liferay.com/images/dxp/latest/en/users-and-permissions/accounts/account-roles/images/01.png)
4. Click the _Define Permissions_ tab at the top of the page. Select what permissions the Role has to act upon the resources owned by the Account, including the Account itself. For example, selecting Update permissions enables the Role to change the Account’s description. Click _Save_ to save the permissions.
5. Click the _Define Group Scope Permissions_ tab. Select what permissions the Role has to act on resources owned by other groups (i.e. Sites, channels, asset libraries, etc.). Note that the permissions apply for the Account that is selected as the current account.
6. To assign an Account User to this Role, navigate to _Control Panel_ → _Accounts_. Select a specific account.
7.  Click the _Roles_ tab and click the _Options_ icon (![Options icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) of the Role you just created. Click _Assign Users_.

    ![Click the options icon of the account and click Assign Users.](https://resources.learn.liferay.com/images/dxp/latest/en/users-and-permissions/accounts/account-roles/images/02.png)
8. Click the _Add_ icon (![Add icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png)) and assign a User to this Role.

#### Creating and Assigning an Owned Account Role <a href="#creating-and-assigning-an-owned-account-role" id="creating-and-assigning-an-owned-account-role"></a>

1. Open the Global Menu (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)). Navigate to _Control Panel_ → _Accounts_.
2. Select the specific account in which to create the new Role. Click the _Roles_ tab and click the _Add_ icon (![Add icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png)).
3. Give the new Role a title and description. Click _Save_ to create the Role.
4.  Click the _Define Permissions_ tab at the top of the page. Select what permissions the Role has to act upon the resources owned by the Account, including the Account itself. For example, selecting the Update permissions enables the Role to change the Account’s description. Click _Save_ to save the permissions.

    ![Select the permissions to assign to this role.](https://resources.learn.liferay.com/images/dxp/latest/en/users-and-permissions/accounts/account-roles/images/03.png)
5. Click the _Define Group Scope Permissions_ tab. Select what permissions the Role has to act on resources owned by other groups (i.e. Sites, channels, asset libraries, etc.). Note that the permissions apply for the Account that is selected as the current account.
6. To assign an Account User, click the _Assignees_ tab. Click the _Add_ icon (![Add icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png)) and assign a User to this Role.
