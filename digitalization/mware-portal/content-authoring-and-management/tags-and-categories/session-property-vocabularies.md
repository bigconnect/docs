# Session Property Vocabularies

With a _Session Property Vocabulary_, you can create [session-based Segment properties](https://learn.liferay.com/web/guest/w/dxp/site-building/personalizing-site-experience/segmentation/segments-editor-ui-reference#session-properties) using predefined values. This option is useful when you don’t want the Segment editor to enter the properties’ values manually while creating a new Segment; instead, you want the user to select them from a list of predefined values. Session Property Vocabularies facilitate the task of defining Segments and eliminate manual entry errors.

For example, your Marketing team may use a device’s brand information to target different content to your website visitors, like the promotion of a new app for Apple devices. To facilitate the process of the Marketing team, you can create a Session Property Vocabulary that includes the different device brands and link this Vocabulary to the [user Segment](https://learn.liferay.com/web/guest/w/dxp/site-building/personalizing-site-experience/segmentation/creating-and-managing-user-segments). This way, when the Marketing team makes a Segment using the “Device Brand” [session property](https://learn.liferay.com/web/guest/w/dxp/site-building/personalizing-site-experience/segmentation/segments-editor-ui-reference#session-properties), they don’t need to type the different Device Brand options. The list of possible options is now predefined for them.

![The Device Brand session's property contains a list of pre-defined values.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/tags-and-categories/session-property-vocabularies/images/07.png)

You work with Session Property Vocabularies in three steps:

1. [Configure the Vocabulary for the session-based Segment](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/tags-and-categories/session-property-vocabularies#configuring-the-vocabulary-for-the-session-based-segment).
2. [Define the Session Property Vocabulary](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/tags-and-categories/session-property-vocabularies#defining-the-session-property-vocabulary).
3. [Create the session-based Segment](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/tags-and-categories/session-property-vocabularies#creating-the-session-based-segment).

### [Configuring the Vocabulary for the Session-Based Segment](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/tags-and-categories/session-property-vocabularies#configuring-the-vocabulary-for-the-session-based-segment) <a href="#configuring-the-vocabulary-for-the-session-based-segment" id="configuring-the-vocabulary-for-the-session-based-segment"></a>

The categories you define on this Vocabulary (1) are the ones you can choose when you create a Segment using the [session properties](https://learn.liferay.com/web/guest/w/dxp/site-building/personalizing-site-experience/segmentation/segments-editor-ui-reference#session-properties) (2).

![Here's the list of categories for the global Vocabulary you want to associate to the user Segment.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/tags-and-categories/session-property-vocabularies/images/01.png)

![The available options in the session-based Segment corresponds to the Categories in the global Vocabulary.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/tags-and-categories/session-property-vocabularies/images/02.png)

You can create a new global Vocabulary or use an existing one. To create a new Vocabulary or update the Categories on an existing one, see [Defining Categories and Vocabularies for Content](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/tags-and-categories/defining-categories-and-vocabularies-for-content). You can use vocabularies with [Public or Internal Visibility](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/tags-and-categories/organizing-content-with-categories-and-tags#vocabulary-visibility). Session Property Vocabularies only work with global vocabularies.

NOTE

You can only create and edit global vocabularies in the global Site.

### [Defining the Session Property Vocabulary](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/tags-and-categories/session-property-vocabularies#defining-the-session-property-vocabulary) <a href="#defining-the-session-property-vocabulary" id="defining-the-session-property-vocabulary"></a>

After [configuring the Vocabulary for the session-based Segment](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/tags-and-categories/session-property-vocabularies#configuring-the-vocabulary-for-the-session-based-segment), you must link the Vocabulary to one of the available session properties in a new Session Property Vocabulary.

![Configuring the Vocabulary to work with the Segment's Session Property.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/tags-and-categories/session-property-vocabularies/images/03.png)

1. Click the global Menu and click the _Control Panel_ tab.
2. Under the Configuration section, click _System Settings_.
3. In the Content and Data section, click _Segments_.
4. Click _Session Property Vocabularies_.
5. Click _Add_ to create a new Session Property Vocabulary.
6.  Select the property in the Selection Property drop-down menu.

    ![Select the property you want to associate to the global Vocabulary.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/tags-and-categories/session-property-vocabularies/images/06.png)

    NOTE

    You cannot select properties that belong to another Session Property Vocabulary. These properties are grayed-out and unavailable.
7. Select the _Vocabulary_ in the Vocabulary Name drop-down menu.
8. Click _Save_.

![Create a Session Property Vocabulary and save it.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/tags-and-categories/session-property-vocabularies/images/05.gif)

### [Creating the Session-Based Segment](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/tags-and-categories/session-property-vocabularies#creating-the-session-based-segment) <a href="#creating-the-session-based-segment" id="creating-the-session-based-segment"></a>

After [configuring the Vocabulary](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/tags-and-categories/session-property-vocabularies#configuring-the-vocabulary-for-the-session-based-segment) and [defining the Session Property Vocabulary](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/tags-and-categories/session-property-vocabularies#defining-the-session-property-vocabulary), you can create the Segment. For information on creating segments, see [Creating and Managing User Segments](https://learn.liferay.com/web/guest/w/dxp/site-building/personalizing-site-experience/segmentation/creating-and-managing-user-segments#creating-user-segments).

When you define the Segment’s conditions and you use one of the session properties linked to the global Vocabulary, the conditions for the session property are now predefined and you can select them from a drop-down menu. This drop-down menu corresponds the list of categories for [the Vocabulary you define](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/tags-and-categories/session-property-vocabularies#defining-the-session-property-vocabulary) in the Session Property Vocabulary.

IMPORTANT

When you delete one of the categories or the Vocabulary that is linked to a User Segment in a Session Property Vocabulary, the Segment keeps working based on your definiton. Liferay alerts you of this situation in the Segment editor.

![A warning message appears when one of the categories or the Vocabulary in the Session Property Vocabulary is missing.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/tags-and-categories/session-property-vocabularies/images/08.png)
