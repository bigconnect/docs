# Creating Web Content Templates

Web Content Templates use the FreeMarker Template Language (FTL) to determine how content fields are rendered on a Page. Each Template can be associated with a Web Content [Structure](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/web-content/web-content-structures/creating-structures), or even [embedded in other Templates](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/web-content/web-content-templates/embedding-widgets-in-templates#embedding-other-templates).

IMPORTANT

Grant permission to Create or Edit templates in FreeMarker to trusted users only. See [Assigning Permissions to Web Content Structures and Templates](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/web-content/web-content-structures/assigning-permissions-to-structures-and-templates#security-considerations-for-web-content-templates) for information about Web Content Templates security.

### [How to Create a Web Content Template](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/web-content/web-content-templates/creating-web-content-templates#how-to-create-a-web-content-template) <a href="#how-to-create-a-web-content-template" id="how-to-create-a-web-content-template"></a>

1. Open the _Site Menu_ (![Site Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-menu.png)), and go to _Content & Data_ → _Web Content_.
2. Select the _Templates_ tab, and click on the _Add button_ (![Add Template](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png)) to create a new Web Content Template.
3. Enter a title for your new Template.
4.  Optionally, you can determine these configurable [Template Properties](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/web-content/web-content-templates/creating-web-content-templates#configurable-template-properties):

    ![Optionally, determine Template settings in the Properties panel.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/web-content/web-content-templates/creating-web-content-templates/images/01.png)
5.  Add FreeMarker code to your Template via the script window, or by clicking on _Choose File_ to select an existing `.ftl` script file to import.

    When manually entering code in the script window, you can quickly access standard variables in the left Template panel. If you’ve selected a Structure for your Template, you can also access variables for its specific fields, as well as the _Friendly URL_ variable.

    You can then add HTML or additional FreeMarker code to your Template’s script.

    ![Access standard variables in the left Template panel, as well as variables specific to your selected Structure.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/web-content/web-content-templates/creating-web-content-templates/images/02.png)
6.  Click on _Save_ when finished or _Save and Continue_ to save your progress and continue editing.

    IMPORTANT

    Once your Template is linked with a Structure and saved, you can no longer change this Template property.

    When saved, DXP automatically generates a DDM Template Key, URL, and WebDAV URL for your Template, which you can view in its Properties panel.

    ![DXP automatically generates a DDM Template Key, URL, and WebDAV URL for your Template.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/web-content/web-content-templates/creating-web-content-templates/images/03.png)

Once you’ve created a Template, you can [use it for Web Content](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/web-content/web-content-articles/adding-a-basic-web-content-article) created with its linked Structure or [embed it in other Templates](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/web-content/web-content-templates/embedding-widgets-in-templates#embedding-other-templates), depending on its configuration. You can also edit your Template at any time.

NOTE

Changes in a Structure may require changes in associated Templates. For example, if a Structure field name referenced in a Template is renamed or removed, the Template’s reference must be updated manually.

### [Configurable Template Properties](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/web-content/web-content-templates/creating-web-content-templates#configurable-template-properties) <a href="#configurable-template-properties" id="configurable-template-properties"></a>

While Creating a template, you can optionally determine the following Template settings in the Properties panel:

* **Structure**: Use the Structure field to link your Template with an existing Web Content Structure, or leave this field blank to create a generic Template that can be embedded in other Web Content Templates.
* **Description**: Use the Description field to provide localized descriptions of your Template.
* **Cacheable**: By default, new Templates are cacheable. Uncheck this property if dynamic behavior is needed (e.g., [taglibs](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/web-content/web-content-templates/using-taglibs-in-templates), request handling, CMS query API).
* **Featured Image**: Select a featured image for your Template via URL or from your device.

\
