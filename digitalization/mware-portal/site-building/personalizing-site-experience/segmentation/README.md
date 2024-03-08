# Segmentation

Using _Segments_ you can create different groups of users based on a common set of properties, like user _Job Title_, or user _Language_. For a list of the properties you can use to define Segments, see [Segments Editor UI Reference](https://learn.liferay.com/web/guest/w/dxp/site-building/personalizing-site-experience/segmentation/segments-editor-ui-reference). In addition to the exiting properties, you can add [Custom Fields](https://learn.liferay.com/web/guest/w/dxp/system-administration/configuring-liferay/adding-custom-fields) for User and Organization groups and use these fields on you Segment criteria.

### [Compound Segments](https://learn.liferay.com/web/guest/w/dxp/site-building/personalizing-site-experience/segmentation/creating-and-managing-user-segments#compound-segments) <a href="#compound-segments" id="compound-segments"></a>

Liferay DXP/Portal 7.3+

You can combine existing Segments to create a new _Compound Segment_. The Compound Segment inherits the properties from the Segments it’s based on and updates the members automatically. You can add additional properties to further customize the Compound Segment.

To understand how Compound Segments work, consider the following example. Visitors from the USA and Canada have different needs, so you create two Segments with different properties, one Segment for each country. Later, you create a new standard Segment for North America visitors, where you manually copy the properties of the USA and Canada Segments. When you change the properties of the USA or Canada Segments, the North America Segment doesn’t inherit the changes. You must manually update the North America segment to reflect these changes. However, if you create the North America segment as a Compound Segment, this Compound Segment automatically updates its definition and members when you change the USA or Canada Segments.

IMPORTANT

You cannot use an existing Compound Segment to create a new one.

![Combine two or more segments to create a new Compound Segment](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/personalizing-site-experience/segmentation/creating-and-managing-user-segments/images/08.png)

### [Creating User Segments](https://learn.liferay.com/web/guest/w/dxp/site-building/personalizing-site-experience/segmentation/creating-and-managing-user-segments#creating-user-segments) <a href="#creating-user-segments" id="creating-user-segments"></a>

These steps describe how to create a new Segment:

1.  From the Site Menu on the left side of the screen, go to _People_ → _Segments_.

    ![Add User Segments from the People Menu.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/personalizing-site-experience/segmentation/creating-and-managing-user-segments/images/01.png)
2. Click the _Add_ button (![Add](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png)).
3.  Click in the top text area and enter a name for your User Segment.

    TIP

    You can use the flag selector next to the Segment’s name to translate the name of your Segment.
4. From the _Properties_ area, select the group and property to define your Segment. Drag and drop the _Property_ to the _Conditions_ area.
5. Configure the Conditions for your segment (see [Configuring Segment’s Conditions](https://learn.liferay.com/web/guest/w/dxp/site-building/personalizing-site-experience/segmentation/creating-and-managing-user-segments#configuring-segment-conditions) below).
6. Click _Save_.

#### [Configuring Segment Conditions](https://learn.liferay.com/web/guest/w/dxp/site-building/personalizing-site-experience/segmentation/creating-and-managing-user-segments#configuring-segment-conditions) <a href="#configuring-segment-conditions" id="configuring-segment-conditions"></a>

From the _Condition_ area you have these options:

* Edit the comparison criteria using the comparison drop-down menu (A).
* Add or remove _Conditions_ from the same _Properties_ group, using the buttons next to the condition’s name (B).
* Add conditions using a different _Properties_ group, by dragging and dropping the _Property_ (C).
*   Combine the _Conditions_ with _AND_ and _OR_ operators (D and E).

    ![Add and combine Conditions to define the Segment criteria.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/personalizing-site-experience/segmentation/creating-and-managing-user-segments/images/06.png)

For the [Session Properties](https://learn.liferay.com/web/guest/w/dxp/site-building/personalizing-site-experience/segmentation/segments-editor-ui-reference#session-properties), you can configure a list of predefined values using [Session Property Vocabularies](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/tags-and-categories/session-property-vocabularies). This option facilitates the task of defining the segments and eliminates manual entry errors.

NOTE

Starting from Liferay DXP 7.4 U42+/GA42+, you can create Segments by geolocation. See [Segmentation by Geolocation](https://learn.liferay.com/web/guest/w/dxp/site-building/personalizing-site-experience/segmentation/segmentation-by-geolocation) for more information.

As you edit, a count of members meeting the criteria appears at the top of the Conditions area. You can click on _View Members_ to see the list. This helps you determine if you are correctly defining the Segment.

![You can view the list of Segment members at any time.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/personalizing-site-experience/segmentation/creating-and-managing-user-segments/images/04.png)

After you create your User Segment, you can see it in the list of User Segments on the _Segments_ page. From here, you can manage the User Segment (edit it, delete it, [assign Site Roles](https://learn.liferay.com/web/guest/w/dxp/users-and-permissions/roles-and-permissions/assigning-roles-to-user-segments), or change the permissions (who can access the User Segment) for it through its Actions Menu (![Actions](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)). You can also click on the User Segment’s name to edit it.

![You can edit, delete or manage permissions from the actions menu.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/personalizing-site-experience/segmentation/creating-and-managing-user-segments/images/05.png)

NOTE

You can’t delete a User Segment if it’s used in an experience.

\
