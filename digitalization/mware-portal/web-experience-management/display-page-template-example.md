# Display Page Template Example

Create a new Display Page Template:

1. Go to _Site Builder_ → _Pages_ in Site Administration.
2. Go to the _Display Page Templates_ tab.
3. Click the _Add_ button (![Add](https://liferay-support.zendesk.com/hc/article\_attachments/360030160011/icon-add.png)).
4. Name your Display Page _Lunar Resort Display Page Template_, open the _Content Type_ dropdown and select _Web Content Article_ and _Basic Web Content_ for the Subtype, and click _Save_.

![Figure 1: Selecting the Asset type and Subtype.](https://liferay-support.zendesk.com/hc/article\_attachments/360041112311/display-page-asset-type.png)

To build the Display Page Template, you can add any number of Fragments—with and without editable content—to the page to build your design. Fragments with editable content can have their editable fields mapped to be filled by a Web Content article. You can also base it on a specific Web Content Structure.

![Figure 3: Editing a Display Page Template with some Fragments added.](https://liferay-support.zendesk.com/hc/article\_attachments/360030160031/display-page-with-fragments.png)

Figure 3: Editing a Display Page Template with some Fragments added.

Notice that the example has an editable title and text body, with a static footer containing graphics and links. After you’ve added some fragments to the page, you can map them like this:

1. Navigate to the _Section Builder_ → _Basic Components_ tab in the right menu.
2. Add a _Heading 1_ and _Text_ components to the appropriate areas.
3. Click on an editable text area and click the _Map_ button (![Map](https://liferay-support.zendesk.com/hc/article\_attachments/360041108292/icon-map.png)) in the dialog that appears.
4.  Select a field to map the editable fragment to. The mapped field highlights purple to indicate that it’s mapped.

    <img src="https://liferay-support.zendesk.com/hc/article_attachments/360041108312/display-page-map-field.png" alt="Figure 4: Mapping the editable fragments to structure fields." data-size="original">

    Figure 4: Mapping the editable fragments to structure fields.
5. Click _Publish_ at the top of the page to save your work.

You now have a Display Page Template with static graphics and a text area that’s replaced with whatever content you add to it.

**Note:** You can map any data or metadata from a Web Content Article or Structure to a Display Page Template. For the Basic Web Content type, this includes structure-defined fields like Summary, Title, and Content, as well as metadata fields like Publish Date, Categories, and Tags. In a user-defined structure, all user selected fields appear here as well. Custom fields are also available for display if they apply to the content type selected.

For more information on the right panel and the Fragment options available to you, see [Content Page Elements](content-page-elements.md) and Content Page Interface. Content Pages and Display Page Templates share the same Page builder tools.

### Publishing with Display Page Templates <a href="#publishing-with-display-page-templates" id="publishing-with-display-page-templates"></a>

Now create a short article to display with this display page template:

1. Go to _Content & Data_ → _Web Content_ in Site Administration.
2. Add a Basic Web Content article.
3. Name it _Thoughts About Space_ and fill in some short content.
4. Open the _Display Page Template_ section and under the dropdown, select _Specific Display Page Template_. Then click _Select_.
5. Select the _Lunar Resort Display Page Template_ and click _Done_.
6. Click _Publish_.

![Figure 5: Selecting the Asset type and Subtype.](https://liferay-support.zendesk.com/hc/article\_attachments/360030190832/display-page-creating-content.png)

When published, you can view the content at its Friendly URL (you can find the Friendly URL while editing a Web Content article under _Friendly URL_) or when you click on the content in an Asset Publisher with _Asset Link Behavior_ set to _View in Context_.

When editing the article, you can preview what the display page template will look like with the _Preview_ (![Preview Template](https://liferay-support.zendesk.com/hc/article\_attachments/360042094791/icon-preview.png)) button located next to the selected display page template.

![Figure 6: Selecting the Asset type and Subtype.](https://liferay-support.zendesk.com/hc/article\_attachments/360030190812/display-page-in-context.png)

You can go back and edit the display page template by navigating to _Site Administration_ → _Site Builder_ → _Pages_ → _Display Page Templates_ and clicking _Actions_ (![Actions](https://liferay-support.zendesk.com/hc/article\_attachments/360034597851/icon-staging-bar-options.png)) → _Edit_ next to the display page template you want to edit. If you’re viewing the published display page, you can also select the _Edit_ button (![Edit](https://liferay-support.zendesk.com/hc/article\_attachments/360034597871/icon-edit-pencil.png)) from the Control Menu.

Awesome! You now know how to create and configure a display page using display page templates.
