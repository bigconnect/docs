# Creating Segments with Custom Fields and Session Data

Now that you created a segment, you can take things to the next level and use a Custom Field to define segment criteria.

### Creating a Custom Field <a href="#creating-a-custom-field" id="creating-a-custom-field"></a>

First, create a custom field to use for the Segment:

1. In the _Control Panel_ go to _Configuration_ → _Custom Fields_.
2. Click on _User_.
3. Click the _Add_ button (![Add](https://liferay-support.zendesk.com/hc/article\_attachments/360033274452/icon-add.png)) button in the top right.
4. On the next page, click on _Dropdown_.
5. For the _Field Name_ enter _Cardholder Type_.
6. For values, enter _None,_ _Basic_, _Gold_, and _Platinum_ on four separate lines.
7. Click _Save_.

Now any time user is created, they are prompted to enter the _Cardholder Type_, and existing users can select it from their user profiles.

![Figure 1: You can easily create custom fields to capture whatever kind of data you need.](https://liferay-support.zendesk.com/hc/article\_attachments/360029911751/sp-create-custom-field.png)

### Defining a Segment with a Custom Field <a href="#defining-a-segment-with-a-custom-field" id="defining-a-segment-with-a-custom-field"></a>

Next, use a custom field to define another segment.

1. From Segments Administration, click the _Add_ button (![Add](https://liferay-support.zendesk.com/hc/article\_attachments/360033274452/icon-add.png)).
2. Click in the text area at the top of the page, enter the name _Premium Card Prospects_.
3. For User Properties select _Cardholder Type_.
4. Click on the operator field and set it to _equals_.
5. Select _Basic_ from the select box.

![Figure 2: The custom field you created is seamlessly integrated into segment creation.](https://liferay-support.zendesk.com/hc/article\_attachments/360029911731/sp-select-custom-field.png)

Click _Save_ to save your Segment.

As you can see, segment criteria can be easily defined using the portal's built-in criteria or your own custom fields. Now, you’ll use session data to make this Segment definition even more robust.

### Extending a Segment With Session Data <a href="#extending-a-segment-with-session-data" id="extending-a-segment-with-session-data"></a>

So far, you’ve used criteria derived from user profiles to determine if they should be members of a segment; now it’s time to use session data to make your criteria more effective.

**Note:** For this exercise to work, you must have set a cookie on the specified page.

1. Click the _Actions_ button (![Actions](https://liferay-support.zendesk.com/hc/article\_attachments/360033274432/icon-actions.png)) next to the _Premium Card Prospects_ segment and select _Edit_.
2. Click the _Session_ dropdown from the Properties menu.
3. Set the comparator for Session Properties to _Or_.
4. Drag _Cookies_ into the Session Properties section.
5. Change the selection box to _contains_.
6. Enter _Cards_ in the Key text box.

Now any user who has a cookie from visiting the _Cards_ page is evaluated as part of the _Premium Card Prospects_ segment.
