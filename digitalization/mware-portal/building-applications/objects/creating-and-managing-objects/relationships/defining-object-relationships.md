# Defining Object Relationships

Relationships add fields and tables to object definitions for relating their entries to one another. Follow these steps to add relationships to object definitions:

1. Open the _Global Menu_ ( ![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png) ), go to the _Control Panel_ tab, and click _Objects_.
2. Begin editing the desired object definition.
3. Go to the _Relationships_ tab and click _Add_ ( ![Add Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png) ).
4.  Enter a _label_ and _name_.

    **Label**: Identifies the relationship in the Objects UI and can be localized after the relationship is created.

    **Name**: Determines the relationship’s name in the back-end and uses camel case. Once the relationship is published, this value cannot be changed.
5.  Select a relationship type: _One to Many_ or _Many to Many_.

    **One to Many**: Relate one object entry to multiple object entries. This type adds a relationship table to entries on the parent side of the relationship (i.e., the ‘one’ side) and a relationship field to entries on the child side (i.e., the ‘many’ side).

    **Many to Many**: Relate multiple object entries with multiple object entries. This adds a relationship table to entries on both sides of the relationship.

    NOTE

    In the Objects UI, relationship fields appear in a definition’s Fields tab, while tables appear in the Relationships tab.
6.  Select an _object_ to relate to the current object. You can relate objects to themselves or other object definitions.

    ![Enter a label and name, and select both a relationship type and the desired object.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/relationships/defining-object-relationships/images/01.png)
7.  Click _Save_.

    ![View the relationship in the Relationships tab.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/relationships/defining-object-relationships/images/02.png)

Once saved, you can use the relationship to relate entries.

You can edit the relationship to configure its deletion type or make it mandatory.

IMPORTANT

Relationship fields automatically appear in an object’s default layout. Displaying relationship tables requires a custom layout with a Relationships tab. This tab lists all entries related to the current entry. System objects do not support custom layouts, so they cannot display relationship fields or tables. You can only access their relationship fields via REST APIs. See Designing Object Layouts for more information.

### Configuring Deletion Type <a href="#configuring-deletion-type" id="configuring-deletion-type"></a>

After defining a relationship, you can configure its deletion type. This setting determines how entry deletion is handled when an entry relates to another entry.

![After creating a relationship, you can configure its deletion type.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/relationships/defining-object-relationships/images/03.png)

Available options include _Prevent_, _Disassociate_, and _Cascade_.

**Prevent** (Default): Prevent the deletion of parent entries if they are related to child entries.

**Disassociate**: Allow users to delete parent entries while they are related to child entries. Deleting parent entries does not affect related entries.

**Cascade**: Allow users to delete parent entries while they are related to child entries. Deleting parent entries also deletes their related entries if the user has permission to delete entries in the child object.

### Making Relationship Fields Mandatory <a href="#making-relationship-fields-mandatory" id="making-relationship-fields-mandatory"></a>

Version 7.4 +

Objects on the child side of a one-to-many relationship inherit relationship fields. In some scenarios, you may want to make these fields mandatory. For example, if you’re creating a Ticket object, you may want to define a mandatory relationship field for selecting an ‘assignee’. Or if you want to restrict access to object data by account, you should make the object’s relationship with the account object mandatory.

Follow these steps to make a relationship field mandatory:

1. Begin editing the _object_ containing the desired relationship field.
2.  Go to the _Fields_ tab and begin editing the _relationship field_.

    ![Go to the Field tab and begin editing the relationships field](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/relationships/defining-object-relationships/images/04.png)
3.  Toggle the _Mandatory_ switch.

    ![Toggle the Mandatory switch.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/relationships/defining-object-relationships/images/05.png)
4. Click _Save_.

\
