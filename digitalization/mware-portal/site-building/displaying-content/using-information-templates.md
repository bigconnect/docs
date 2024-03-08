# Using Information Templates

With Information Templates, you can create [FreeMarker](https://freemarker.apache.org/) templates for any item type that implements the Info Framework. Supported item types include Web Contents, Documents, Assets, Blogs, Categories, and Objects. Once created, you can map fragment fields to your template to determine how an item is rendered in Content Pages and Display Pages. This can help you implement a consistent style for your Site.

IMPORTANT

Information templates are only available in fragments and are not available in widgets. If you want to design FreeMarker templates for Web Content that’s displayed in widgets, use [Web Content Templates](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/web-content/web-content-templates/creating-web-content-templates).

### [Creating Information Templates](https://learn.liferay.com/web/guest/w/dxp/site-building/displaying-content/using-information-templates#creating-information-templates) <a href="#creating-information-templates" id="creating-information-templates"></a>

Follow these steps to create an Info Template:

1. Open the _Site Menu_ (![Site Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu.png)) and go to _Design_ → _Templates_ → _Information Templates_.
2. Click the _Add_ button (![Add Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png))
3.  Enter a _name_, select an _Item Type_, and (if applicable) select an _Item Subtype_.

    ![Enter a name and select an Item Type and Item Subtype.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/using-information-templates/images/01.png)
4. Click _Save_. This redirects you to the template design page.
5. (Optional) [Import](https://learn.liferay.com/web/guest/w/dxp/site-building/displaying-content/using-information-templates#importing-and-exporting-scripts) an existing FreeMarker script from an `.ftl` file.
6.  Design your template using FreeMarker.

    You can type `${` in the editor to use the autocomplete feature.

    In the _Elements_ side panel (![Elements](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-list-ul.png)), you can quickly access available fields from the selected item type’s structure.

    ![Design your template using FreeMarker and available fields in the Elements side panel.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/using-information-templates/images/02.png)

    NOTE

    The Elements side panel does not include fields based on templates (e.g., Journal Article Templates, Info Templates). This is to prevent infinite loops.
7. (Optional) Add a _description_ or _image_ to the template in the Properties side panel (![Properties](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-cog3.png)).
8. Click _Save_ when finished.

Once created, Info Templates can be used in Content Display fragments and any other fragment with [sub-elements](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-fragments-and-widgets/using-fragments/configuring-fragments/fragment-sub-elements-reference) that can be mapped, such as text and rich text. When selected, they determine how an item is rendered in Content Pages and Display Page Templates.

Liferay’s [Headless](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/using-liferay-as-a-headless-platform) API services are available in the template editor through the `restClient` object. See [Calling Headless APIs in the Template Editor](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-fragments-and-widgets/using-widgets/styling-widgets/creating-a-widget-template#calling-headless-apis-in-the-template-editor) for usage details.

### [Importing and Exporting Scripts](https://learn.liferay.com/web/guest/w/dxp/site-building/displaying-content/using-information-templates#importing-and-exporting-scripts) <a href="#importing-and-exporting-scripts" id="importing-and-exporting-scripts"></a>

When editing an Info Template, you can import and export FreeMarker scripts to reuse in other contexts:

1. Open the _Site Menu_ (![Site Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu.png)) and go to _Design_ → _Templates_ → _Information Templates_.
2. Select the desired Info Template.
3.  Click the _Actions_ button ![Actions Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png) in the Application Bar and select _Export Script_ or _Import Script_.

    ![Select Export Script or Import Script.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/using-information-templates/images/03.png)

When exporting, the current FreeMarker script is saved as a `script.ftl` file.

When importing, the selected file replaces the template’s current FreeMarker script.

### [Managing Info Templates](https://learn.liferay.com/web/guest/w/dxp/site-building/displaying-content/using-information-templates#managing-info-templates) <a href="#managing-info-templates" id="managing-info-templates"></a>

In the Templates application, you can view and manage all existing Info Templates. Each existing template is listed with its type (e.g., Web Content Article) and sub-type (e.g., Basic Web Content). You can click the _Actions_ button (![Actions Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) for a template to access the following options:

| Action      | Description                                                 |
| ----------- | ----------------------------------------------------------- |
| Edit        | Edit the template.                                          |
| Copy        | Create a new template with the selected template’s content. |
| Permissions | Manage basic permissions for the template.                  |
| Delete      | Delete the template.                                        |

![Click the Actions button to access management options.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/using-information-templates/images/04.png)

Here you can also [export](https://learn.liferay.com/web/guest/w/dxp/site-building/displaying-content/using-information-templates#exporting-templates) and [import](https://learn.liferay.com/web/guest/w/dxp/site-building/displaying-content/using-information-templates#importing-templates) Info Templates as LAR files to share them between Sites and environments. To do this, click the _Actions_ button in the Application Bar and and select _Export/Import_.

![Export and Import templates as LAR files.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/using-information-templates/images/05.png)

NOTE

By default, Info Template exports include Widget Templates. However, you can configure which templates are included in the LAR file.

\
