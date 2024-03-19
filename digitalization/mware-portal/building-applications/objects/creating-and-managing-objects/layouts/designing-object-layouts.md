# Designing Object Layouts

Layouts define how fields and relationships appear when creating and editing object entries. Each layout can include multiple tabs containing different elements that appear during or after entry creation. When designing a layout, there are two types of tabs:

* [Field tabs](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/layouts/designing-object-layouts#adding-field-tabs) can display fields created directly in an object definition or inherited from a one-to-many relationship.
* [Relationship tabs](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/layouts/designing-object-layouts#adding-relationship-tabs) can only display relationship tables from one-to-many or many-to-many relationships.

While designing layouts, you must first add a field tab, then you can add other tabs. Only the first tab appears during object entry creation. Other tabs appear when editing the object entry. Once you’ve finished designing your layout, mark it as [default](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/layouts/designing-object-layouts#selecting-a-default-layout) to use it.

IMPORTANT

The first tab must be a field tab. This tab must include all mandatory object fields. Also, you can only add a relationship tab after the layout has a field tab. Relationship tabs cannot be the first tab in an object layout.

### [Adding Layouts](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/layouts/designing-object-layouts#adding-layouts) <a href="#adding-layouts" id="adding-layouts"></a>

1. Open the _Global Menu_ (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)), go to the _Control Panel_ tab, and click _Objects_.
2.  Begin editing the desired object definition.

    NOTE

    System objects do not support custom layouts. See [Extending System Objects](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/extending-system-objects) for more information.
3. Go to the _Layouts_ tab and click _Add_ (![Add Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png)).
4.  Enter a _Name_ and click _Save_.

    ![Go to the Layouts tab and click Add.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/layouts/designing-object-layouts/images/01.png)

This creates a blank layout to which you can add [field](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/layouts/designing-object-layouts#adding-field-tabs) and [relationship](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/layouts/designing-object-layouts#adding-relationship-tabs) tabs.

### [Adding Field Tabs](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/layouts/designing-object-layouts#adding-field-tabs) <a href="#adding-field-tabs" id="adding-field-tabs"></a>

1.  Go to the _Layout_ tab, and click _Add Tab_.

    ![Go to the Layout tab and click Add Tab.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/layouts/designing-object-layouts/images/02.png)
2.  Enter a _Label_, select the _Fields_ type, and click _Save_.

    The label determines the tab’s display name in the Liferay UI.

    ![Enter a Label, select the Fields type, and click Save.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/layouts/designing-object-layouts/images/03.png)
3.  Click _Add Block_, enter a _Label_, and click _Save_.

    This creates a subsection for adding fields. The label determines the section’s display name in the Liferay UI.

    Repeat this step to add multiple blocks to the same tab.

    ![Add blocks to the field tab.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/layouts/designing-object-layouts/images/04.png)
4.  Click _Add Field_ for the desired block, select an object _field_, determine the field’s _size_ (i.e., one, two, or three columns), and click _Save_.

    ![Click Add Field, select a field, determine the field's size, and click Save](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/layouts/designing-object-layouts/images/05.png)

    Repeat this step to add more fields to the block.

    ![Add fields to the block.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/layouts/designing-object-layouts/images/06.png)
5.  Use the toggle switch to make a block collapsible.

    ![Use the toggle switch to make a block collapsible.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/layouts/designing-object-layouts/images/07.png)
6. After adding the desired tabs, save your changes to the layout.

After adding a field tab, you can add categorization tools to it so end users can apply tags and categories to object entries. See [Adding Categorization to Field Tabs](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/layouts/adding-categorization-to-fields-tabs) for more information.

### [Adding Relationship Tabs](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/layouts/designing-object-layouts#adding-relationship-tabs) <a href="#adding-relationship-tabs" id="adding-relationship-tabs"></a>

1.  Edit the desired layout, go to the _Layout_ tab, and click _Add Tab_.

    ![Select the desired layout, go to the Layout tab, and click Add Tab.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/layouts/designing-object-layouts/images/08.png)
2.  Enter a _Label_, select the _Relationships_ type, and select the _relationship table_ you want to display in the tab.

    The label determines the tab’s display name in the Liferay UI.

    ![For tab type, select Relationships and choose a relationship table to display.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/layouts/designing-object-layouts/images/09.png)
3.  Click _Save_.

    ![Add relationship tabs to the layout.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/layouts/designing-object-layouts/images/10.png)
4. After adding the desired tabs, save your changes to the layout.

IMPORTANT

If the object has a many-to-many relationship with itself, you can add two relationship tabs to its layout. Each tab corresponds to one side of the relationship. For example, consider a ticket object that relates multiple tickets to one another. You can define two relationship tabs for keeping track of tickets and their dependencies. One tab would list all tickets that the current ticket depends on, while the other tab would list all tickets that depend on the current ticket. See [Defining Object Relationships](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/relationships/defining-object-relationships) for more information.

### [Selecting a Default Layout](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/layouts/designing-object-layouts#selecting-a-default-layout) <a href="#selecting-a-default-layout" id="selecting-a-default-layout"></a>

Once you’ve designed your layout, you can set it as the object’s default layout. This ensures it is used for creating and editing the object’s entries. Otherwise, the object uses the autogenerated layout.

Follow these steps:

1. Select the desired _layout_.
2.  In the Basic Details tab, check _Mark as Default_.

    ![Check Mark as Default and click Save.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/layouts/designing-object-layouts/images/11.png)
3. Click _Save_.

The layout is now used when creating and editing the object’s entries.

![The object uses the default layout for creating and editing entries.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/layouts/designing-object-layouts/images/12.png)
