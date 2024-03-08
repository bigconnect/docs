# Creating and Managing Asset Libraries

You can create and manage libraries for your assets via the Asset Libraries application. To access this application, open the _Global Menu_ (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)), go to the _Applications_ tab, and click _Asset Libraries_.

![Create and manage asset libraries via the Asset Libraries application.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/asset-libraries/creating-and-managing-asset-libraries/images/01.png)

### [Creating an Asset Library](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/asset-libraries/creating-and-managing-asset-libraries#creating-an-asset-library) <a href="#creating-an-asset-library" id="creating-an-asset-library"></a>

1. In the Asset Library application, click _Add_ (![Add button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png)).
2. Enter a _Name_ for your new library.
3. Click _Save_.

You can now configure the asset library now or accept the default settings and begin adding content to it.

### [Configuring Asset Libraries](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/asset-libraries/creating-and-managing-asset-libraries#configuring-asset-libraries) <a href="#configuring-asset-libraries" id="configuring-asset-libraries"></a>

When you create an asset library, Liferay directs you to its configuration page. Afterwards, you can configure a library by clicking its _Actions_ (![Actions button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) and selecting _Edit_. Configuration options appear in three sections: _General_, _Sites_, and _Languages_.

You can also access these options from the library’s dashboard page by clicking _Asset Library Settings_.

#### [General Settings](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/asset-libraries/creating-and-managing-asset-libraries#general-settings) <a href="#general-settings" id="general-settings"></a>

Here you can configure an asset library’s basic information.

**Details**: View a library’s unique ID and configure its name or description.

**Applications**: Activate or deactivate supported applications for the library (i.e., Documents and Media, Collections, Translation Processes, Web Content). This determines if you can use the application in the library. Deactivating an application does not delete the application’s assets or remove them from connected sites.

**Sharing**: Enable or disable sharing for assets. When enabled, asset library members can share its assets with other users.

**Asset Auto Tagging**: Enable or disable [auto tagging](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/tags-and-categories/auto-tagging/auto-tagging-assets). When enabled, assets are automatically tagged using the providers configured at the instance level.

**Documents and Media**: Enable or disable HTTP directory browsing for this library. By default, this setting is disabled. When enabled, users with view permission for an asset library can view files and folders in its document library in their browsers (e.g., `http://localhost:8080/documents/asset-library-{assetLibraryId}`). For Liferay 2024.Q1+/GA105+, also set a max file size for uploaded assets. This value cannot exceed the maximum upload request size set at the instance or system levels. Setting the value to 0 sets no limit for uploaded files.

**Recycle Bin**: Enable or disable Recycle Bin for a library, and set the maximum age for recycled assets. By default, the maximum age is set to 43200 minutes (i.e., 30 days).

#### [Sites](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/asset-libraries/creating-and-managing-asset-libraries#sites) <a href="#sites" id="sites"></a>

Here you can connect the asset library to Liferay sites and configure their relationship.

![Connect sites to an asset library and configure their connection.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/asset-libraries/creating-and-managing-asset-libraries/images/02.png)

**Add**: Connect a new site to the selected library.

**Make Searchable/Unsearchable**: Determine whether the library’s assets appear in search queries for the selected site. By default, all connections are searchable.

**Make Web Content Structures Available**: By default, this feature is disabled. When enabled, you can use the library’s web content structures and templates in connected sites. If you later disable this feature, web content using the library’s structures and templates in a site is no longer available.

**Disconnect**: Disconnect the selected site from the library. When disconnected, the library’s assets are no longer accessible from the site. You cannot disconnect a library from a site while _Make Web Content Structures Available_ is enabled. This is to avoid breaking site content that uses the library’s structures and templates.

#### [Languages](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/asset-libraries/creating-and-managing-asset-libraries#languages) <a href="#languages" id="languages"></a>

Here you can choose whether to use your instance’s default language configuration for the library or define your own. If you choose a custom configuration, click _Edit_ to determine which languages are available in the library. Then you can drag and drop languages to set their relative priority.

![Use your instance's default language options for an asset library, or define your own](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/asset-libraries/creating-and-managing-asset-libraries/images/03.png)

To set a custom default language, click the desired language’s _Actions_ (![Actions button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) and select _Make Default_.

### [Managing Asset Library Memberships](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/asset-libraries/creating-and-managing-asset-libraries#managing-asset-library-memberships) <a href="#managing-asset-library-memberships" id="managing-asset-library-memberships"></a>

You can view and manage user memberships for an asset library by going to its dashboard page and clicking _Memberships_. From here, you can click _Add_ (![Add button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png)) to add new users, organizations, and user groups to the selected library. You can also assign roles or remove memberships for each existing member by clicking its _Actions_ (![Actions button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)).

![View an manage asset library memberships.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/asset-libraries/creating-and-managing-asset-libraries/images/04.png)

By default, asset libraries include these roles:

* **Asset Library Member**: Grants basic permissions and is automatically assigned to users who belong to an asset library.
* **Asset Library Connected Site Member**: Automatically assigned to members of a connected site.
* **Asset Library Content Reviewer**: Grants permissions necessary for reviewing content in a workflow.
* **Asset Library Administrator**: Grants permission to manage most aspects of an asset library, including content, memberships, and settings. However, it does not grant permission to modify or assign the Owner or Administrator roles to other members.
* **Asset Library Owner**: Grants control over all aspects of an asset library and is automatically assigned to the library’s creator.

NOTE

By default, the asset library member’s permissions are more restricted than those of its site counterpart. This means you must manually grant access to applications, update/delete permissions for content, and more if you want members to be responsible for managing content.

Creating or customizing an asset library role is the same process as for other kinds of roles. See [Roles and Permissions](https://learn.liferay.com/web/guest/w/dxp/users-and-permissions/roles-and-permissions) for how to create, customize, manage, and assign roles.

### [Deleting an Asset Library](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/asset-libraries/creating-and-managing-asset-libraries#deleting-an-asset-library) <a href="#deleting-an-asset-library" id="deleting-an-asset-library"></a>

To remove an asset library and its contents from your instance, click its _Actions_ (![Actions button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) and select _Delete_. You are then prompted to confirm the deletion. Once deleted, all contained assets are removed from your database and are no longer available to previously connected sites.

WARNING

Deleting asset libraries cannot be undone. Ensure you’re removing the correct library and have backed up any assets you want to keep.

\
