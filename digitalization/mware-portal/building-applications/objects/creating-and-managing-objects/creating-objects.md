# Creating Objects

Liferay Objects provides development capabilities in the Liferay UI. Using Objects, you can build fully integrated Liferay applications without writing any code or manually deploying modules. This process includes [creating](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/creating-objects#creating-object-drafts) object drafts, [editing](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/creating-objects#editing-object-drafts) them, and finally [publishing](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/creating-objects#publishing-object-drafts) them to create the new application.

Here you’ll learn how to create object definitions using the default Objects UI, but you can also create, manage, and extend object definitions using the Model Builder GUI. See [Using the Objects Model Builder](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/using-the-objects-model-builder) for more information.

IMPORTANT

Liferay creates database tables when you publish object definitions, so you cannot use Objects with the high security database practices described in [Database Configurations](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/reference/database-configurations#high-security-database-user-practices).

### [Creating Object Drafts](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/creating-objects#creating-object-drafts) <a href="#creating-object-drafts" id="creating-object-drafts"></a>

Object drafts are application templates that store the necessary data for creating an object application. To create a draft object,

1.  Open the _Global Menu_ (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)), go to the _Control Panel_ tab, and click _Objects_.

    NOTE

    For Liferay 7.4 2023.Q4+/GA102+, you must also have the _Object Folder > View_ permissions to access the Objects UI. See [Objects Application Permissions](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/objects-application-permissions) for more information.
2. Go to the desired [object folder](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/organizing-objects-with-folders) and click the _Add_ button (![Add Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png)).
3.  Provide these details for the draft.

    **Label**: Identifies the object in the Objects admin and supported application contexts (e.g., Workflow, Display Page Templates, Forms).

    **Plural Label**: Determines the display name for the object application in the UI.

    **Object Name**: Determines the object’s `definition.name` and cannot be changed after publishing.

    ![Enter a Label, Plural Label, and Name for the object draft.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/creating-objects/images/01.png)
4. Click _Save_.

Once saved, a blank object draft is created without any configurations or data elements. You can view the draft in the Objects admin alongside system objects. To finish the creation process, you must [edit](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/creating-objects#editing-object-drafts) the draft and then [publish](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/creating-objects#publishing-object-drafts) it to your Liferay instance.

![You can view the new object draft in the Objects admin.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/creating-objects/images/02.png)

### [Editing Object Drafts](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/creating-objects#editing-object-drafts) <a href="#editing-object-drafts" id="editing-object-drafts"></a>

Liferay 7.4 2023.Q3+/GA92+

Before publishing an object, you can add elements and configure its settings. This includes adding fields, defining validations, setting a scope for its data, and more.

NOTE

Whenever you add a field, relationship, or layout, the object draft is saved automatically. However, any unsaved changes in the Details tab are lost if you navigate to other tabs.

Follow these steps to edit the object draft:

1.  From the Objects admin page, click the newly created object draft to begin editing it.

    Object features and configuration options are organized into the following tabs: Details, Fields, Relationships, Layouts, Actions, Views, Validations, and State Manager.

    ![Object features and configuration options are organized into eight tabs.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/creating-objects/images/03.png)
2.  Click the _Fields_ tab to add custom fields. Each field represents a database column for storing specific types of values. See [Adding Fields to Objects](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/fields/adding-fields-to-objects).

    By default, all objects include these system fields: Author, Create Date, External Reference Code, ID, Modified Date, and Status.

    ![Add fields to the object from the Fields tab.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/creating-objects/images/04.png)
3.  (Optional) Click the _Relationships_ tab to add relationships. Relationships are connections between object definitions that you can use to link their entries. See [Defining Object Relationships](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/relationships/defining-object-relationships).

    ![Add relationships to the object from the Relationships tab.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/creating-objects/images/05.png)
4.  (Optional) Click the _Layouts_ tab to add layouts. Object layouts determine how fields and relationships appear when creating and viewing object entries. See [Designing Object Layouts](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/layouts/designing-object-layouts).

    NOTE

    If you do not add a custom layout and set it as default, the object uses an automatically generated layout for its entries. This layout has a single tab that displays all object fields in alphabetical order and does not display relationships. To display relationships, you must create a custom layout with a dedicated relationships tab.

    ![Add custom layouts to the object from the Layouts tab.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/creating-objects/images/06.png)
5.  (Optional) Click the Actions tab to add custom actions. Each action defines an operation that runs automatically according to a set trigger. See [Defining Object Actions](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/actions/defining-object-actions).

    ![Add custom actions to the object from the Actions tab.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/creating-objects/images/07.png)
6.  (Optional) Click the Views tab to add custom views. Object views determine how entries are displayed in the object’s application page. See [Designing Object Views](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/views/designing-object-views).

    ![Add custom views to the object from the Views tab.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/creating-objects/images/08.png)
7.  (Optional) Click the Validations tab to add validations. Each validation sets rules for determining valid field entries. See [Adding Field Validations](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/validations/adding-field-validations).

    ![Add custom validations to the object from the Validations tab.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/creating-objects/images/09.png)
8.  (Optional) If you’ve added a state field to the draft, click the State Manager tab to define its flow. See [Adding and Managing Custom States](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/fields/adding-and-managing-custom-states).

    ![Define flows for state fields in the State Manager tab.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/creating-objects/images/10.png)
9. Click the Details tab to configure the object draft.
10. (Optional) Change the name, label, or plural label for the object.

    ![Change the name, label and plural label for the object.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/creating-objects/images/11.png)
11. Select an Entry Title. This determines the field used to identify the object’s entries in the UI (e.g., relationship fields). By default, objects use the ID field.

    ![Once a field is added to an object, you can select it as the Entry Title.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/creating-objects/images/12.png)
12. Select an object _scope_. This determines how the object’s data is stored.

    **Company** (default): When scoped by Company, the object’s data is stored per Liferay instance.

    **Site**: When scoped by Site, the object’s data is stored per Site.
13. Select a Panel Link to determine where the definition appears in Liferay.

    For company-scoped objects, select tab and category in the Global Menu (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)).

    For site-scoped objects, select a section in the Site Menu (![Site Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-menu.png)).

    You can leave this value blank if you don’t want to display the application in a UI menu.

    ![Select a scope and panel link for the object.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/creating-objects/images/13.png)
14. Configure account restriction to restrict access to object entries by account. You can only configure this setting before publishing.

    This option only appears if the object is on the child side of a one-to-many relationship with the Account system object. See [Restricting Access to Object Data by Account](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/using-system-objects-with-custom-objects/restricting-access-to-object-data-by-account).

    ![Determine whether to limit restrict access to object entries by account.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/creating-objects/images/14.png)
15. Show or hide the object’s page widget.

    While enabled, you can add the object’s widget to site pages. See [Deploying Object Widgets in Sites](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/deploying-object-widgets-to-sites).
16. Enable or disable categorization for object entries.

    While enabled, you can add categorization blocks to fields tabs in custom layouts. See [Adding Categorization to Fields Tabs](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/layouts/adding-categorization-to-fields-tabs). Additionally, you can attach metadata to the object entries by adding the tags and categories fragments in form containers.
17. Enable or disable comments for object display pages.

    While enabled, you can use the Page Comments widget in entry display pages. See [Creating Display Page Templates for Objects](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/displaying-object-entries#creating-display-page-templates-for-objects).
18. Enable or disable history for object entries.

    While enabled, Liferay keeps a history of entry events that you can view in the Audit application or via REST API calls. See [Auditing Object Events](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/auditing-object-events).
19. [Beta Feature](https://learn.liferay.com/web/guest/w/dxp/system-administration/configuring-liferay/feature-flags#beta-feature-flags) Enable or disable draft mode for object entries.

    While enabled, users can save entries as a draft and finalize them at a later time. Saving drafts does not trigger custom validations, workflow processes, or actions triggered by entry creation.

    NOTE

    You can configure this setting at any time. Disabling draft mode does not change the status of existing entries.
20. Enable or disable translation support for custom text [fields](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/fields).

    While enabled, you can enable translation for custom text fields, so end users can localize their input. See [Localizing Object Definitions and Entries](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/localizing-object-definitions-and-entries) for more information.
21. Click _Save_.

Once you’ve finished configuring and editing the draft, you can [publish](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/creating-objects#publishing-object-drafts) it.

### [Publishing Object Drafts](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/creating-objects#publishing-object-drafts) <a href="#publishing-object-drafts" id="publishing-object-drafts"></a>

Publishing an object creates and activates your new application. When activated, users can access it in the Liferay UI according to its scope and panel link. Before publishing your object, review the following information to understand the consequences of publishing a draft.

#### [What Happens During Publishing](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/creating-objects#what-happens-during-publishing) <a href="#what-happens-during-publishing" id="what-happens-during-publishing"></a>

The publishing process includes the following operations:

* A database table is created for the object with the draft’s data definitions.
* A new Headless API is automatically created for CRUD operations.
* A collection provider is created for displaying the object’s entries.
* A page widget is created for the object.
* The object is integrated with Info framework, so you can select the object as a content type for a [display page template](https://learn.liferay.com/web/guest/w/dxp/site-building/displaying-content/using-display-page-templates)
* The object is integrated with the [Permissions framework](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/understanding-object-integrations/permissions-framework-integration), so you can manage permissions for the new application and its resources.
* The object is integrated with the Workflow framework, so you can configure a custom workflow for the object.
* The object is integrated with Forms, so you can select the object as a data storage option.

#### [Configuration Restrictions for Published Objects](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/creating-objects#configuration-restrictions-for-published-objects) <a href="#configuration-restrictions-for-published-objects" id="configuration-restrictions-for-published-objects"></a>

Liferay 7.4 U86+/GA86+

To prevent data loss and system conflicts, Liferay restricts these configuration options for object definitions after publishing:

* Name
* Scope
* Account Restriction (if enabled)
* Entry Translation

Liferay also restricts some custom field options (e.g., mandatory, unique values).
