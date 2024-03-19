# Using the Objects Model Builder

Liferay Objects provides the Model Builder GUI for designing complex object structures. This graphical tool displays each object definition as a card and visualizes relationships between object definitions. With it, you can create, manage, and extend data models in the Objects application and quickly configure definitions, fields, and relationships.

![Use the Model Builder GUI to design complex data structures.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/using-the-objects-model-builder/images/01.png)

To access the Model Builder view,

1. Open the _Global Menu_ (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)), go to the _Control Panel_ tab, and click _Objects_.
2.  Go to the desired [object folder](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/organizing-objects-with-folders) and click _View in Model Builder_.

    ![Go to the desired object folder and click View in Model Builder.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/using-the-objects-model-builder/images/02.png)

From here you can create, edit, and manage object definitions.

TIP

The Model Builder view is great for quickly configuring multiple object definitions and their attributes. It is especially helpful for visualizing and managing object relationships.

### [Creating Objects](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/using-the-objects-model-builder#creating-objects) <a href="#creating-objects" id="creating-objects"></a>

1. Go to the desired object folder.
2.  Click _Create New Object_.

    ![Click Create New Object.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/using-the-objects-model-builder/images/03.png)
3.  Provide these details for your object draft.

    **Label**: Identifies the object in the Objects admin and supported application contexts (e.g., Workflow, Display Page Templates, Forms).

    **Plural Label**: Determines the display name for the object application in the UI.

    **Object Name**: Determines the object’s `definition.name` and cannot be changed after publishing.
4. Click _Save_.

Once saved, the draft definition appears in the center diagram area. You can click it to access configuration options in the right side panel.

![Click an object card to access configuration options.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/using-the-objects-model-builder/images/04.png)

In the side panel, you can edit the object’s details, enable integrations, and configure additional features (e.g., categorization, draft mode).

You can also add fields and relationships to object definitions. However, some configurations cannot be added from the model builder view. [Validations](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/validations), [actions](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/actions), [views](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/views), [layouts](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/layouts), and [states](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/fields/adding-and-managing-custom-states) must be added from the object’s page view. To access the page view from the model builder view, click _Actions_ (![Actions](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) → _Edit in page view_ in the object card.

#### [Adding Data Fields](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/using-the-objects-model-builder#adding-data-fields) <a href="#adding-data-fields" id="adding-data-fields"></a>

1. Click _Add Field or Relationship_ for an object definition.
2.  Select _Add Field_.

    ![Select Add Field.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/using-the-objects-model-builder/images/05.png)
3.  Enter a _Label_ and _Field Name_.

    **Label**: Identifies the field in the Objects UI and can be localized after the field is created.

    **Field Name**: Determines the field’s name in the back-end and uses camel case. Once a field is published, this value cannot be changed.
4. Select a field _Type_. See [Field Types Reference](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/fields/adding-fields-to-objects#field-types-ui-reference) for more information about available field types and configuration options.
5. Click _Save_.

The field is added immediately to the definition. By default, object cards display five fields, but you can click _Show All Fields_ to view others. Additionally, you can select a field in the object card to access its configuration options in the right side panel.

![Select a field to access its configuration options in the right side panel.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/using-the-objects-model-builder/images/06.png)

#### [Defining Relationships](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/using-the-objects-model-builder#defining-relationships) <a href="#defining-relationships" id="defining-relationships"></a>

1. Click _Add Field or Relationship_ for an object definition.
2.  Select _Add Relationship_.

    ![Select Add Relationship.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/using-the-objects-model-builder/images/07.png)
3.  Enter a _Label_ and _Name_.

    **Label**: Identifies the relationship in the Objects UI and can be localized after the relationship is created.

    **Name**: Determines the relationship’s name in the back-end and uses camel case. Once the relationship is published, this value cannot be changed.
4. Select a relationship _Type_ and an object definition for the other side of the relationship. See [Defining Object Relationships](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/relationships/defining-object-relationships) for more information.
5. Click _Save_.

Once saved, the diagram area visualizes the logical connection between object definitions and distinguishes between one-to-many and many-to-many relationships. Click a relationship connector to access configuration options in the right side panel.

![Select a relationship to access its configuration options in the right side panel.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/using-the-objects-model-builder/images/08.png)

If you define relationships with object definitions in other folders, the diagram area displays their cards with a dashed border. This is only for visualizing the relationship and does not grant users the ability to edit the object definitions. To edit a definition, you must navigate to the object’s source folder.

![The diagram area displays related object definitions in other folders.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/using-the-objects-model-builder/images/09.png)

### [Managing Objects](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/using-the-objects-model-builder#managing-objects) <a href="#managing-objects" id="managing-objects"></a>

You can drag and drop object cards in the Model Builder diagram area to reorganize them visually. Liferay saves position updates automatically. Additionally, you can move object definitions between folders.

1. Go to the desired destination folder.
2. Click _Actions_ (![Actions Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) for an object from another folder in the left sidebar.
3.  Select _Move to Current Folder_.

    ![Select Move to Current Folder](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/using-the-objects-model-builder/images/10.png)

This moves the definition to the current folder. Moving an object definition does not affect its relationships and is only for improving organization and object management.

![Move object definitions between folders.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/using-the-objects-model-builder/images/11.png)

You can access additional management options by clicking Actions (![Actions Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) for an object card. Available options include

* Edit in Page View: Switch to the default Objects UI.
* Edit ERC: Update the object’s external reference code (ERC).
* Export as JSON: [Download](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/exporting-and-importing-object-definitions) the JSON object definition.
* Manage Permissions: Determine which roles can [act on the object](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/objects-application-permissions#actions-on-database-entities).
* Delete Object: [Delete](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/deleting-objects) the object definition.

![Click the object card's Actions button to access additional management options.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/using-the-objects-model-builder/images/12.png)

#### [Publishing Object Drafts](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/using-the-objects-model-builder#publishing-object-drafts) <a href="#publishing-object-drafts" id="publishing-object-drafts"></a>

While using Model Builder, you can publish individual object definitions as well as complex data models that include multiple objects.

1. Go to the folder with the object drafts.
2. Click _Publish_.
3.  Select which drafts to publish.

    ![Select which drafts to publish and click Publish Objects.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/using-the-objects-model-builder/images/13.png)
4. Click _Publish Objects_.

The selected drafts begin publishing simultaneously.

\
