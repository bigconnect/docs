# Creating User Segments

To learn how to use segmentation, you’ll step through an example for defining two segments for a site. The segments use user data like the _Job Title_ field and organization membership for evaluating segments. The first Segment you’ll create, _American Engineers_, uses standard fields as criteria.

To get started, navigate to the Segments page.

1. Go to _Site Administration_ for the Bank site.
2. Open the _People_ section and select _Segments_.

### Creating a Custom Segment <a href="#creating-a-custom-segment" id="creating-a-custom-segment"></a>

On the Segments page, you’ll see a list of all the currently available segments, if they’re available. Create a segment named _American Engineers_ for employees of your company.

1. Click the _Add_ button (![Add](https://liferay-support.zendesk.com/hc/article\_attachments/360033274412/icon-add.png)) button.
2. Click in the top text area and enter the name _American Engineers_.
3. Open the _User_ tab in the right side _Properties_ menu and drag the _Job Title_ property to the _Conditions_ area.
4. Click on the operator field and set it to _contains_.
5.  Click on the text field and enter _Engineer_.

    <img src="https://liferay-support.zendesk.com/hc/article_attachments/360029927852/sp-set-date.png" alt="Figure 1: Setting the comparator to contains includes variations of Engineer like Software Engineer in the segment." data-size="original">

    Figure 1: Setting the comparator to \*contains\* includes variations of "Engineer" like "Software Engineer" in the segment.
6. Open the _Organization_ properties and drag over an _Organization_ field.
7. Set the comparator between _User Properties_ and _Organization Properties_ to _And_.
8.  Set the _Organization_ field to be _equals_ and select the organization.

    <img src="https://liferay-support.zendesk.com/hc/article_attachments/360033274392/sp-select-orgs.png" alt="Figure 2: You can prevent typos by directly selecting Organizations through the interface." data-size="original">

    Figure 2: You can prevent typos by directly selecting Organizations through the interface.
9. Click _Save_ to save your Segment.

As you edit, a count of members meeting the criteria appears at the top of the page. You can click on _View Members_ to see the list. This helps you determine if you are correctly defining the Segment.

![Figure 3: You can view the list of Segment members at any time.](https://liferay-support.zendesk.com/hc/article\_attachments/360033265571/sp-segment-members.png)

### Managing Segments <a href="#managing-segments" id="managing-segments"></a>

After you create your Segment, you can see it in the list of Segments on the _People_ → _Segments_ page. From there you can edit the segment, delete it, or change the permissions for it. You cannot delete a segment if it’s being used in an experience. Also, changing permissions only affects who has access to manage the Segment; it doesn’t change Segment membership or criteria.

1. Go to _People_ → _Segments_.
2. Click the _Actions_ button (![Actions](https://liferay-support.zendesk.com/hc/article\_attachments/360033265531/icon-actions.png)) next to the _American Engineers_.

You can manage the options here. You can also click on the Segment’s name to edit it.

![Figure 4: You can edit, delete or manage permissions from the options menu.](https://liferay-support.zendesk.com/hc/article\_attachments/360029911711/sp-options.png)

Next, you’ll define a Custom Field and use it as part of your Segmentation criteria.
