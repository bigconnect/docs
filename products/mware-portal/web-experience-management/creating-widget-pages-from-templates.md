# Creating Widget Pages from Templates

Page templates provide pre-configured pages to reuse.&#x20;

Three sample layout page templates are installed by default:

* **Search:** Contains a search bar and configuration to display various facets.
* **Wiki:** Provides a page with three applications related to authoring a wiki.
* **Blog:** Provides a page with three applications related to blogging.

![Figure 1: The Blog page template is already available for use along with the Search and Wiki page templates.](https://liferay-support.zendesk.com/hc/article\_attachments/360030188732/default-page-templates.png)

To add a new widget page template,

1. Go to _Site Builder_ → _Pages_.
2. Select the _Page Templates_ tab.
3. Click _New_ and create a collection named _Lunar Resort Templates_.
4. Click the _Add_ icon (![Add Page Template](https://liferay-support.zendesk.com/hc/article\_attachments/360030188752/icon-add.png)) and select _Widget Page Template_.
5. Enter a name.
6. Click _Save_.

The editing page for the template appears. You can add widgets to the page or access page configuration now. The changes you make are instantly applied to the template.

If you want to edit the template again,

1. Go back to the _Page Templates_ tab.
2. Click the _Actions_ icon (![Actions](https://liferay-support.zendesk.com/hc/article\_attachments/360030157751/icon-actions.png)).
3. Click _Configure_.

Note that after a new page template has been created, the default permissions only allow the creator to use the page template. To give other users access to it,

1. Use the Actions menu for the template and select _Permissions_.
2. In the matrix of Roles and permissions, check the _View_ permission for the Roles needed to see the page template in the list of available page templates when creating a new page.

If you want any user who can create a page to be able to use the page template, check the _View_ permission for the _User_ Role.

To use your template to create a new page,

1. Go to Site Administration and select the _Pages_ option from the _Site Builder_ menu dropdown option.
2. Click the _Add_ icon (![Add Page](https://liferay-support.zendesk.com/hc/article\_attachments/360030188752/icon-add.png)).
3. Inside the Lunar Resort collection, select the page template that you created.
4. Enter the name of your page and click _Add_.

Pages based on templates can inherit changes from the page template:

![Figure 2: You can choose whether or not to inherit changes made to the page template.](https://liferay-support.zendesk.com/hc/article\_attachments/360030188792/automatic-application-page-template-changes.png)

By default, when a Site administrator creates pages based on a page template, future changes to the template are automatically propagated to those pages. Site administrators can disable this behavior by disabling the _Inherit Changes_ selector. Occasionally, propagation for page templates fails due to unintended errors.&#x20;

If staging has been enabled, changes to the page template are automatically propagated to the staged page. These changes must still be approved before the page is published to live. For this reason, the automatic propagation of page template changes to the staged page cannot be turned off and the _Inherit Changes_ selector does not appear.

### Sharing Widget Page Templates <a href="#sharing-widget-page-templates" id="sharing-widget-page-templates"></a>

When importing pages to a new site or environment, you must also import templates associated with those pages. Generally templates are included automatically when an associated page is exported, but if not you can export the template collection separately so the page can be imported to the new environment. To export page templates,

1. Go to _Site Management_ → _Site Builder_ → _Pages_.
2. Select the _Page Templates_ tab.
3. At the top right of the page, click _Options_ (![Options](https://liferay-support.zendesk.com/hc/article\_attachments/360030188772/icon-options.png)) → _Export/Import_.
4. On the _Export/Import_ page you can choose to export configuration data and change which Collections and templates are being exported.
5. When you’re done configuring the export, click _Export_ and save the exported LAR file.
6. On the target environment, go to _Site Management_ → _Site Builder_ → _Pages_ and select _Page Templates_.
7. At the top right of the page, click _Options_ (![Options](https://liferay-support.zendesk.com/hc/article\_attachments/360030188772/icon-options.png)) → _Export/Import_.
8. Select the _Import_ tab.
9. Upload the LAR with your template data. If the LAR contains additional content you don’t want to import, you can deselect it.

Once the template has been imported, the page can be imported normally to your new environment
