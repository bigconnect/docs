# Managing Roles

You manage Roles and Permissions in the Control Panel (_Control Panel_ → _Users_ → _Roles_). There you’ll find an application for creating Roles, granting them permissions, and assigning Users to them. Roles can be scoped by portal, Site, or Organization.

![Figure 1: The Roles application lets you add and manage roles for the global (Regular), Site, or Organization scope.](https://liferay-support.zendesk.com/hc/article\_attachments/360030187452/roles-app.png)

### Creating Roles <a href="#creating-roles" id="creating-roles"></a>

Determine the scope of the Role you must create. Roles can be scoped globally (Regular Roles), to a specific Site (Site Roles), or to an Organization (Organization Roles).

To create a Role:

1. Click the tab for the proper Role scope, then click the _Add_ (![Add](https://liferay-support.zendesk.com/hc/article\_attachments/360030187432/icon-add.png)) button.
2. Enter a title and description. The title field is required but the description is optional.
3. Enter a Key, if desired. This required field provides a key that can be used to refer to the Role programmatically. It’s auto-populated with the title text, but you can override it if desired.
4. Click _Save_.

Now the Role is present in the database and ready for further configuration.

### Assigning Users to a Role <a href="#assigning-users-to-a-role" id="assigning-users-to-a-role"></a>

Assign users to a Role in the Assignees tab of the Add/Edit Role form. Roles are assigned to Users, Sites, Organizations, or User Groups. Here’s how to assign the User Group Manager Role created in the last section to Users:

1. In the Assignees tab of the Add/Edit Role form, click the second level tab for _Users_.
2. Click the Add button (![Add](https://liferay-support.zendesk.com/hc/article\_attachments/360030187432/icon-add.png)).
3. Select the Users you want to add to the Role and click _Add_.

If assigning a group, note that all Users assigned to that group inherit the Role as well.

That’s a good start, but your Role isn’t worth the database row it occupies without defining permissions for the Role. Read the next article to learn how.
