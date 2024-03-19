# Objects Application Permissions

The Objects application is integrated with the permissions framework, so you can assign [application](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/objects-application-permissions#application-permissions) and [resource](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/objects-application-permissions#resource-permissions) permissions to user roles to determine which users can access the application and its data.

![Assign Objects permissions when defining role permissions.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/objects-application-permissions/images/01.png)

See [Defining Role Permissions](https://learn.liferay.com/web/guest/w/dxp/users-and-permissions/roles-and-permissions/defining-role-permissions) for more information about assigning permissions to user roles.

### [Application Permissions](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/objects-application-permissions#application-permissions) <a href="#application-permissions" id="application-permissions"></a>

Application permissions grant permission to perform general application-related operations and do not include [resource-related permissions](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/objects-application-permissions#resource-permissions).

Objects has the following application permissions:

![Objects has the following application permissions.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/objects-application-permissions/images/02.png)

| Permission              | Description                                                        |
| ----------------------- | ------------------------------------------------------------------ |
| Access in Control Panel | Access Objects in the Global Menu.                                 |
| Configuration           | View and modify configuration options for the Objects application. |
| Permissions             | View and modify Objects permissions.                               |
| Preferences             | View and modify preferences for the Objects application.           |
| View                    | View the Objects application.                                      |

IMPORTANT

For Liferay 7.4 2023.Q4+/GA102+, users must have the _Object Folder > View_ permission to use the Objects UI.

### [Resource Permissions](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/objects-application-permissions#resource-permissions) <a href="#resource-permissions" id="resource-permissions"></a>

Resource permissions grant access to view, create, and act on definitions and folders in the Objects application. Some of these permissions are for [creating entities](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/objects-application-permissions#object-definitions-resource-related-actions) while others are for acting on existing entities (e.g., [definitions](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/objects-application-permissions#object-definition-for-existing-custom-definitions) and [folders](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/objects-application-permissions#object-folder-for-existing-object-folders)).

Objects has the following resource permissions.

#### [Object Definitions (Resource-Related Actions)](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/objects-application-permissions#object-definitions-resource-related-actions) <a href="#object-definitions-resource-related-actions" id="object-definitions-resource-related-actions"></a>

![Manage resource-related actions in the Objects application.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/objects-application-permissions/images/03.png)

| Permission                      | Description                                                            |
| ------------------------------- | ---------------------------------------------------------------------- |
| Add Object Definition           | Create an object definition draft.                                     |
| Add Object Folder               | Create a folder for organizing object definitions.                     |
| Extend System Object Definition | Add fields, relationships, actions, and validations to system objects. |
| Permissions                     | View and manage permissions related to objects.                        |
| Publish Object Definition       | Publish an object definition draft.                                    |

#### [Object Definition (For Existing Custom Definitions)](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/objects-application-permissions#object-definition-for-existing-custom-definitions) <a href="#object-definition-for-existing-custom-definitions" id="object-definition-for-existing-custom-definitions"></a>

![Manage permissions for existing object definitions.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/objects-application-permissions/images/04.png)

| Permission  | Description                                           |
| ----------- | ----------------------------------------------------- |
| Delete      | Delete an object definition.                          |
| Permissions | View and modify permissions for an object definition. |
| Update      | Update an object definition.                          |
| View        | View an object definition.                            |

#### [Object Folder (For Existing Object Folders)](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/objects-application-permissions#object-folder-for-existing-object-folders) <a href="#object-folder-for-existing-object-folders" id="object-folder-for-existing-object-folders"></a>

![Manage permissions for existing object folders.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/objects-application-permissions/images/05.png)

| Permission            | Description                                       |
| --------------------- | ------------------------------------------------- |
| Add Object Definition | Move an object definition to the folder           |
| Delete                | Delete an object folder.                          |
| Permissions           | View and modify permissions for an object folder. |
| Update                | Update an object folder.                          |
| View                  | View an object folder.                            |

### [Managing Permissions for Individual Object Definitions](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/objects-application-permissions#managing-permissions-for-individual-object-definitions) <a href="#managing-permissions-for-individual-object-definitions" id="managing-permissions-for-individual-object-definitions"></a>

1. Open the _Objects_ application via the _Global Menu_ (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)).
2.  Click the _Actions_ button (![Actions Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) for the desired object definition and select _Permissions_.

    ![Click the Actions button for the desired object definition and select Permissions.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/objects-application-permissions/images/06.png)
3.  Use the checkboxes to grant [object definition](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/objects-application-permissions#object-definition-for-existing-custom-definitions) permissions to the desired roles.

    NOTE

    Permissions defined at the roles admin level override permissions defined at the individual level.

    ![Use the checkboxes to assign permissions to the desired roles.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/objects-application-permissions/images/07.png)
4. Click _Save_.

### [Managing Permissions for Individual Folders Definitions](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/objects-application-permissions#managing-permissions-for-individual-folders-definitions) <a href="#managing-permissions-for-individual-folders-definitions" id="managing-permissions-for-individual-folders-definitions"></a>

1. Open the _Objects_ application via the _Global Menu_ (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)).
2.  Click _Actions_ (![Actions Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) for the desired object folder and select _Permissions_.

    ![Click the Actions button for the desired object folder and select Permissions.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/objects-application-permissions/images/08.png)
3.  Use the checkboxes to grant [object folder](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/objects-application-permissions#object-folder-for-existing-object-folders) permissions to the desired roles.

    NOTE

    Permissions defined at the roles admin level override permissions defined at the individual level.

    ![Use the checkboxes to assign permissions to the desired roles.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/objects-application-permissions/images/09.png)
4. Click _Save_.
