# Managing Picklist Permissions

Picklist is integrated with Liferay’s permissions framework. This means you can assign [application](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/picklists/managing-picklist-permissions#application-permissions) and [resource](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/picklists/managing-picklist-permissions#resource-permissions) permissions to user roles to determine which users have access to the Picklists application and its data.

![Assign Picklist permissions when defining Role permissions.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/picklists/managing-picklist-permissions/images/01.png)

See [Defining Role Permissions](https://learn.liferay.com/web/guest/w/dxp/users-and-permissions/roles-and-permissions/defining-role-permissions) for more information about assigning permissions to user roles.

### [Application Permissions](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/picklists/managing-picklist-permissions#application-permissions) <a href="#application-permissions" id="application-permissions"></a>

Application permissions grant permission to perform general application-related operations and do not include [resource-related permissions](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/picklists/managing-picklist-permissions#resource-permissions).

Picklists has the following application permissions:

| Permission              | Description                                                         |
| ----------------------- | ------------------------------------------------------------------- |
| Access in Control Panel | Access Picklists in the Global Menu                                 |
| Configuration           | View and modify configuration options for the Picklists application |
| Permissions             | View and modify Picklists permissions                               |
| Preferences             | View and modify preferences for the Picklists application           |
| View                    | View the Picklists application                                      |

### [Resource Permissions](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/picklists/managing-picklist-permissions#resource-permissions) <a href="#resource-permissions" id="resource-permissions"></a>

Resource permissions grant permissions on specific application resources. Some of these permissions enable performing [operations on database entities](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/picklists/managing-picklist-permissions#resource-related-actions) (i.e., model resources). Others grant permission to perform [resource-related operations](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/picklists/managing-picklist-permissions#actions-on-database-entities) in an application context (e.g., create a new resource entity).

The Picklists application has the following resource permissions.

#### [Resource-Related Actions](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/picklists/managing-picklist-permissions#resource-related-actions) <a href="#resource-related-actions" id="resource-related-actions"></a>

| Permission               | Description                                      |
| ------------------------ | ------------------------------------------------ |
| Add List Type Definition | Create a Picklist type definition                |
| Permissions              | View and manage permissions related to Picklists |

#### [Actions on Database Entities](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/picklists/managing-picklist-permissions#actions-on-database-entities) <a href="#actions-on-database-entities" id="actions-on-database-entities"></a>

| Permission  | Description                                |
| ----------- | ------------------------------------------ |
| Delete      | Delete a Picklist                          |
| Permissions | View and modify permissions for a Picklist |
| Update      | Update a Picklist                          |
| View        | View a Picklist                            |

### [Managing Permissions for Individual Picklists](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/picklists/managing-picklist-permissions#managing-permissions-for-individual-picklists) <a href="#managing-permissions-for-individual-picklists" id="managing-permissions-for-individual-picklists"></a>

Follow these steps to manage permissions for individual Picklists:

1. Open the _Picklists_ application via the _Global Menu_ (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)).
2.  Click the _Actions_ button (![Actions Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) for the desired Picklist and select _Permissions_.

    ![Click the Actions button for the desired Picklist and select Permissions.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/picklists/managing-picklist-permissions/images/02.png)
3.  Use the checkboxes to grant [database entity](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/picklists/managing-picklist-permissions#actions-on-database-entities) permissions to the desired roles.

    NOTE

    Permissions defined at the roles admin level override permissions defined at the individual level.

    ![Use the checkboxes to assign permissions to the desired roles.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/picklists/managing-picklist-permissions/images/03.png)
4. Click _Save_.

\
