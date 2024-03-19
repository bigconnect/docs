# Displaying Object Entries

With Liferay Objects, you can use existing DXP frameworks to display Object entries dynamically in Sites. This includes collection displays for multiple entries and display pages for individual entries.

When an Object is published, Liferay creates a Collection Provider for use with the [Collection Display fragment](https://learn.liferay.com/web/guest/w/dxp/site-building/displaying-content/collections-and-collection-pages/displaying-collections#adding-a-collection-display-fragment-to-a-page) to display multiple Object entries. You can also configure the fragment to style, filter, and paginate the collection. See [Displaying Object Entries with Collection Providers](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/displaying-object-entries-with-collection-providers) for more information.

And since published Objects are integrated with the Info framework, you can select them as the content type for [Display Page Templates](https://learn.liferay.com/web/guest/w/dxp/site-building/displaying-content/using-display-page-templates). When selected, the Object is set as the template’s mapping source, so you can easily map fragment fields to Object fields to create flexible page displays for individual Object entries.

Additionally, you can use FreeMarker to create [Information Templates for Objects](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/displaying-object-entries#creating-information-templates-for-objects). Once created, you can map fragment fields to your template to determine how entries are rendered in Content Pages and Display Pages.

NOTE

Information Templates can be used in Content Display fragments and any other fragment with [sub-elements](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-fragments-and-widgets/using-fragments/configuring-fragments/fragment-sub-elements-reference) (e.g., text, rich text) that can be mapped to Objects.

Together, these integrations enable users to leverage Liferay’s WYSIWYG page building capabilities while delivering personalized user experiences.

### [Creating Display Page Templates for Objects](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/displaying-object-entries#creating-display-page-templates-for-objects) <a href="#creating-display-page-templates-for-objects" id="creating-display-page-templates-for-objects"></a>

When created, each Object entry is assigned automatically a dedicated friendly URL that uses the following pattern: `server-address/l/objectEntryId` (e.g., `https://mysite.com/l/44337`). To view an entry at its URL, you must first create a Display Page Template for the Object.

Follow these steps:

1. Open the _Site Menu_ (![Site Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-menu.png)), expand _Design_, and go to _Page Templates_.
2. Click the _Display Page Templates_ tab.
3.  Click the _Add_ button (![Add Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png)) and select the _Blank_ template.

    This opens a modal window.
4.  Enter a _name_ and select the desired _Object_ for Content Type.

    ![Enter a name and select the desired Object for Content Type](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/displaying-object-entries/images/01.png)
5.  Click _Save_.

    This creates a blank template with the selected Object as its default mapping source and redirects you to the template editing page.
6.  Use page fragments and widgets to create flexible, dynamic displays for Object entries. See [Using Display Page Templates](https://learn.liferay.com/web/guest/w/dxp/site-building/displaying-content/using-display-page-templates) for information.

    ![Map fragment elements to Object fields to dynamically populate Display Pages for individual entries.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/displaying-object-entries/images/02.png)
7.  Add the Page Comments widget so end users can comment on entry display pages.

    NOTE

    You must enable comments for the object definition. Otherwise, the widget is hidden in the display page.
8. When finished, click _Publish_.
9.  Click the _Actions_ button (![Actions Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) for your template and select _Mark as Default_.

    This template is now used to render the Object’s entries whenever users visit their friendly URLs.

    ![Click the Actions button for the template and select Mark as Default.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/displaying-object-entries/images/03.png)

#### [Enabling Comments](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/displaying-object-entries#enabling-comments) <a href="#enabling-comments" id="enabling-comments"></a>

1. Open the _Global Menu_ (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)), go to the _Control Panel_ tab, and click _Objects_.
2. Edit an object definition.
3.  In the Details tab, enable comments and click _Save_.

    ![Enable comments.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/displaying-object-entries/images/04.png)

### [Creating Information Templates for Objects](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/displaying-object-entries#creating-information-templates-for-objects) <a href="#creating-information-templates-for-objects" id="creating-information-templates-for-objects"></a>

1. Open the _Site Menu_ (![Site Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu.png)) and go to _Design_ → _Templates_ → _Information Templates_.
2. Click _Add_ (![Add Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png))
3.  Enter a _name_ and select the desired _Object_.

    ![Enter a name and select the desired Object.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/displaying-object-entries/images/05.png)
4. Click _Save_. This redirects you to the template design page.
5.  (Optional) Click Actions (![Actions Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) in the Application Bar, select _Import Script_, and choose an `.ftl` file to import. This replaces the current template with the file’s FreeMarker script.

    ![Import script from an .ftl file.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/displaying-object-entries/images/06.png)
6.  Use [FreeMarker](https://freemarker.apache.org/) to design your template.

    In the _Elements_ side panel (![Elements](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-list-ul.png)), you can access available fields from the selected Object’s structure.

    ![Select from available fields in the Object's structure.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/displaying-object-entries/images/07.png)
7.  (Optional) Add a description or image to the template in the Properties side panel (![Properties](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-cog3.png)).

    ![Add a description or image in the Properties panel.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/displaying-object-entries/images/08.png)
8. Click _Save_ when finished.

You can now select from the template when mapping Object entries to fragment fields.

![Select the template when mapping fragment elements.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/displaying-object-entries/images/09.png)

### [Adding Entries to Navigation Menus](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/displaying-object-entries#adding-entries-to-navigation-menus) <a href="#adding-entries-to-navigation-menus" id="adding-entries-to-navigation-menus"></a>

Since Objects are integrated with the info framework, you can add Object entries to navigation menus to provide quick access. All published Objects automatically appear as available sources for menu elements. See [Using the Navigation Menus Application](https://learn.liferay.com/web/guest/w/dxp/site-building/site-navigation/using-the-navigation-menus-application) for more information.

\
