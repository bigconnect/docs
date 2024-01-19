# Publishing Web Content

In the previous sections, you created and edited an article. Now it’s time to publish it.

1. Go to the _Welcome_ page of the Lunar Resort Site.
2. Select the _Add_ button (![Add](https://liferay-support.zendesk.com/hc/article\_attachments/360030159511/icon-add.png)) from the top Control Menu and select the _Widgets_ tab.
3. Find _Web Content Display_ and add it to the page.

![Figure 1: Add the Web Content Display app to a page to begin displaying your new web content article.](https://liferay-support.zendesk.com/hc/article\_attachments/360030159491/add-web-content-display.png)

You can drag a widget to the position on the page where you want your content to appear. You can have as many Web Content Display widgets on a page as you need, which gives you the power to lay out your content exactly the way you want it.

Now select the content to display:

1. Click _Select Web Content to make it visible_ in the bottom of the widget.
2. Click _Select_ under _Web Content_.
3.  Click on the article that you want to display

    If your content does not immediately appear in the list, you can search for the content by title, description, user name, or Site (click the drop-down arrow to see all the options).

Selecting a web content article displays the Web Content Display’s configuration page, where you can choose the User Tools and Content Metadata to be published in the widget. These two entities have the following options to choose from, by default:

* **User Tools**
  * _Translations_
  * _Print_
* **Content Metadata**
  * _Related Assets_
  * _Ratings_
  * _Comments_
  * _Comment Ratings_

If you have enabled OpenOffice/LibreOffice integration, you can also enable document conversion for your content. Then users can download your content in their format of choice. To enable OpenOffice/LibreOffice integration, go to _Control Panel_ → _Configuration_ → _System Settings_ → _Connectors_ and check the _Server Enabled_ box. Back in the Web Content Display’s configuration page, conversion options are available under the _User Tools_ list.

![Figure 2: Publishing web content is a snap. At a minimum, you only have to select the content you wish to publish. You can also enable lots of optional features to let your users interact with your content.](https://liferay-support.zendesk.com/hc/article\_attachments/360030190432/web-content-choosing-web-content.png)

**Translations:** Shows the available locales for your content. If you’re working on the page for a particular language, you can select the translation of your content that goes with your locale.

**Print:** Opens the content in a separate browser window with just the content—no navigation or other widgets.

By default, guests cannot leave comments on web content. If you want to allow guests to comment on your web content article,

1. Navigate to the Control Panel → _Users_ → _Roles_
2. Select _Guest_ → _Define Permissions_.
3. From the left menu, select _Site Administration_ → _Content & Data_ → _Web Content_.
4. Navigate down to the Web Content Article heading and select the _Add Discussion_ checkbox. Click _Save_.

Guests can now post comments on your web content article!

You may decide you want one, some, or none of these features, which is why they’re all implemented as simple selector buttons to be enabled or disabled at need. Once you’ve selected the features you want to include in your Web Content Display widget, click _Save_ and close the configuration window.

### Editing Published Content <a href="#editing-published-content" id="editing-published-content"></a>

If you must edit published content, you can do it directly from the Web Content Display app or from Site Administration. To edit it from the Web Content Display app,

1. Select the _Options_ button (![Options](https://liferay-support.zendesk.com/hc/article\_attachments/360030190452/icon-app-options.png)) from the widget’s top panel.
2. Select _Edit Web Content_ to launch the editor. Select _Edit Template_ to launch the template editor for the web content article’s template if it has one.

If you edit the article from Site Administration, you can also view the article’s history and use the diff tool to compare versions.

1. Go to _Content & Data_ → _Web Content_ from the Product Menu.
2. Next to the article, click _Actions_ icon (![Actions](https://liferay-support.zendesk.com/hc/article\_attachments/360030159471/icon-actions.png)) and select _View History_. This shows you

This shows you all the article’s versions and modified/display dates. The diff tool compares these versions and highlights the differences between them.

1. Click _Actions_ next to a version of the article you’d like to compare.
2. Select _Compare to…_.
3.  Select the version with which to compare it.

    The tool provides color coded highlighting to emphasize additions and deletions between the two articles.

![Figure 3: Comparing web content articles is a great feature to use during the Workflow process.](https://liferay-support.zendesk.com/hc/article\_attachments/360030159531/web-content-diff-feature.png)

Whenever you publish updates to a web content article that’s already being displayed, the content is immediately updated unless you have a workflow enabled.
