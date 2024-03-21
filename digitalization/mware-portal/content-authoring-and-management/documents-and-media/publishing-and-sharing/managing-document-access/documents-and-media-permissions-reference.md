# Documents and Media Permissions Reference

Manage access to Documents and Media assets using application and resource permissions. With role-based access control, you can ensure only the intended users can create, view, download, and manage documents.

You can grant these permissions in the Roles application under Content & Data in the permissions side panel.

![Grant Documents and Media permissions in the Roles application under Content & Data in the permissions side panel.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/publishing-and-sharing/managing-document-access/documents-and-media-permissions-reference/images/01.png)

Alternatively, you can assign individual resource permissions in the Documents and Media application.

![Assign individual resource permissions in the Documents and Media application.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/publishing-and-sharing/managing-document-access/documents-and-media-permissions-reference/images/02.png)

See Defining Role Permissions for more information.

### General Permissions <a href="#general-permissions" id="general-permissions"></a>

General permissions grant access to the Documents and Media application itself and do not grant resource-related permissions.

| Permission                                      | Description                                                                                                                                                                           |
| ----------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Access in Site and Asset Library Administration | Access the Documents and Media application in the Site Menu ( ![Site Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu.png) ) or Asset Library. |
| Configuration                                   | View and modify configuration options for Documents and Media.                                                                                                                        |
| Permissions                                     | View and modify application permissions for Documents and Media.                                                                                                                      |
| Preferences                                     | N/A                                                                                                                                                                                   |
| View                                            | View the Documents and Media application page.                                                                                                                                        |

### Resource Permissions <a href="#resource-permissions" id="resource-permissions"></a>

Resource permissions grant access to view and act on resources in the Documents and Media application. Some of these permissions are for creating entities, while others are for acting on existing entities (e.g., folders, documents).

#### Documents <a href="#documents" id="documents"></a>

| Permission        | Description                                                                                    |
| ----------------- | ---------------------------------------------------------------------------------------------- |
| Add Document      | Add documents in Documents and Media.                                                          |
| Add Document Type | Add document types in Documents and Media.                                                     |
| Add Folder        | Add folders in Documents and Media.                                                            |
| Add Repository    | Add repositories in Documents and Media.                                                       |
| Add Shortcut      | Add shortcuts (links) to files and external videos in Documents and Media.                     |
| Add Metadata Set  | Add metadata sets in Documents and Media.                                                      |
| Permissions       | View and modify permissions related to creating and managing resources (e.g., folders, files). |
| Subscribe         | Subscribe to receive email notifications for resource events.                                  |
| Update            | Edit resources in Documents and Media.                                                         |
| View              | View resources in Documents and Media.                                                         |

#### Documents Folder <a href="#documents-folder" id="documents-folder"></a>

| Permission    | Description                                                                              |
| ------------- | ---------------------------------------------------------------------------------------- |
| Access        | Access folder contents.                                                                  |
| Add Document  | Add documents to folders.                                                                |
| Add Shortcut  | Add shortcuts (links) to any file in folders that the role is authorized to view.        |
| Add Subfolder | Add sub-folders to folders.                                                              |
| Delete        | Move folders to the recycle bin.                                                         |
| Permissions   | View and modify folder permissions.                                                      |
| Subscribe     | Subscribe to receive email notifications when files are added to or modified in folders. |
| Update        | Edit or move folders.                                                                    |
| View          | View folders.                                                                            |

#### Document <a href="#document" id="document"></a>

| Permission        | Description                                                                                                                                                                                                          |
| ----------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Add Discussion    | Add comments to documents.                                                                                                                                                                                           |
| Delete            | Move documents to the recycle bin.                                                                                                                                                                                   |
| Delete Discussion | Delete comments from documents.                                                                                                                                                                                      |
| Download          | Download documents.                                                                                                                                                                                                  |
| Override Checkout | Make changes to documents even when checked out by another user.                                                                                                                                                     |
| Permissions       | View and modify document permissions.                                                                                                                                                                                |
| Update            | Edit or move documents.                                                                                                                                                                                              |
| Update Discussion | Edit document comments.                                                                                                                                                                                              |
| View              | View documents. Users with this permission can view all document metadata (e.g., title, description, adaptive media images, previews, thumbnails) and anything defined in attached document types and metadata sets. |

IMPORTANT

For version 7.4 U75/GA75 and earlier versions, the view permission grants users permission to download assets. For 7.4 U76+/GA76+, Documents and Media includes a separate download permission, so you can manage view and download access separately. When upgrading, the portal automatically assigns the download permission to all roles with the view permission. By default all users, including unauthenticated users, can download files. If you want to limit download access, you must remove this permission from roles that should not be able to download files.

#### Shortcut <a href="#shortcut" id="shortcut"></a>

| Permission  | Description                           |
| ----------- | ------------------------------------- |
| Delete      | Move shortcuts to the recycle bin.    |
| Permissions | View and modify shortcut permissions. |
| Update      | Edit or move shortcuts.               |
| View        | View shortcuts.                       |

#### Document Type <a href="#document-type" id="document-type"></a>

| Permission  | Description                                     |
| ----------- | ----------------------------------------------- |
| Delete      | Permanently remove document types.              |
| Permissions | View and modify permissions for document types. |
| Update      | Edit document types.                            |
| View        | View document types.                            |

#### Document Metadata <a href="#document-metadata" id="document-metadata"></a>

| Permission  | Description                                    |
| ----------- | ---------------------------------------------- |
| Delete      | Permanently remove metadata sets.              |
| Permissions | View and modify permissions for metadata sets. |
| Update      | Edit metadata sets.                            |
| View        | View metadata sets.                            |
