# Managing Structures

You can manage structures from the structures page:

1. Open the _Site Menu_ ( ![Site Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu.png) ) and go to _Content & Data_ → _Web Content_.
2. Select the _Structures_ tab.
3. Open the structure’s _Actions Menu_ ( ![Actions Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png) ) and select one of the [available actions](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/web-content/web-content-structures/managing-structures#available-actions).
4. Click _Save_ to apply your changes.

### [Available Actions](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/web-content/web-content-structures/managing-structures#available-actions) <a href="#available-actions" id="available-actions"></a>

You can perform these actions from the structure’s Actions Menu ( ![Actions Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png) ):

**Edit:** Opens the structure so you can update it. See [Configuring Structure Fields](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/web-content/web-content-structures/configuring-structure-fields) for more information.

**Edit Default Values:** Opens a form to set default values for the structure’s fields. See [Editing Default Values](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/web-content/web-content-structures/managing-structures#editing-default-values) for more information.

**Manage Templates:** Opens the Templates tab with a filtered set of web content templates that use the structure. From here you can manage (edit, copy, delete, and set permissions for) the web content templates. See [Managing Templates](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/web-content/web-content-templates/creating-web-content-templates) for more information.

**Make a Copy:** Opens a form to copy the structure and set its name. See [Copying an Existing Structure](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/web-content/web-content-structures/managing-structures#copying-an-existing-structure) for more information.

**Export as JSON:** Exports the structure as a JSON file.

**Import and Override:** Opens an import structure modal. Select and import a JSON file to override the selected structure.

**Permissions:** Opens the permissions configuration dialog for the structure. See [Assigning Permissions to Web Content Structures and Templates](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/web-content/web-content-structures/assigning-permissions-to-structures-and-templates) for more information.

**Delete:** Deletes the structure.

### [Editing Default Values](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/web-content/web-content-structures/managing-structures#editing-default-values) <a href="#editing-default-values" id="editing-default-values"></a>

You can set the default values for a structure’s fields. For example, a structure used for confidential memos may want to include the _confidential_ tag for all the articles created with that structure, or you may want to make the content of all confidential memos non-searchable. From the Structures page, open the _Actions Menu_ ( ![Actions Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png) ) for the structure and follow these steps:

1. Choose _Edit Default Values_.
2.  In the Structure Default Values page, configure the default values for your fields.

    TIP

    You can restore the original values of the structure at any time by clicking the _Reset Values_ button at the top.
3. Click _Save_ to apply the changes.

### [Copying an Existing Structure](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/web-content/web-content-structures/managing-structures#copying-an-existing-structure) <a href="#copying-an-existing-structure" id="copying-an-existing-structure"></a>

You can copy an existing structure to create a new one. From the structures page, open the _Actions Menu_ ( ![Actions Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png) ) for the structure and follow these steps:

1. Choose _Copy_.
2. Type a new name for the structure and, optionally, a new description.
3. Optionally check the _Copy Templates_ box if you want to copy the web content templates associated with the structure.
4. Click _Copy_.

TIP

Liferay DXP generates a unique ID for the copied structure. The new copy inherits all the attributes from the original structure, including the name. To avoid confusing the copy with the original, use a different name for the copy.

\
