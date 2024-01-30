# Displaying Content Sets

Content Sets are primarily displayed through the Asset Publisher. It is currently the only method to display them out of the box, but you can develop your own external applications or widgets to utilize Content Sets. In [Creating Content Sets](creating-content-sets.md) you created two Content Sets. Now display them on a page.

### Configuring the Asset Publisher for Content Sets <a href="#configuring-the-asset-publisher-for-content-sets" id="configuring-the-asset-publisher-for-content-sets"></a>

To display the Content Sets, start with a blank page, and then add the necessary Asset Publishers and configure them to display the Content Sets.

1. Create a new _Home_ page for your site as a Widget Page with a 1 column layout. If you’re using a fresh portal bundle, you can just remove the _Hello World_ widget from the sample _Home_ page.
2. Open the _Add_ menu and add two _Content Management_ → _Asset Publishers_ to the page stacked vertically.
3. Click ![Options](https://liferay-support.zendesk.com/hc/article\_attachments/360030190932/icon-app-options.png) → _Configuration_ for the top Asset Publisher.
4. Under _Asset Selection_ choose _Content Set_.

![Figure 1: The Asset Publisher has a number of options available for selecting its source for content.](https://liferay-support.zendesk.com/hc/article\_attachments/360030191012/content-set-asset-selection.png)

Open _Select Content Set_ and click _Select_.

1. Click on the _Space Program Images_ Content Set.
2. Click _Save_.

Now the images will appear at the top of the page. You can manage the way the content is displayed—like what metadata appears—or even create a _Widget Template_ to style the content, but the items which display and the order in which they appear are determined by the Content Set.

Now configure the bottom Asset Publisher with the other Content Set.

1. Click ![Options](https://liferay-support.zendesk.com/hc/article\_attachments/360030190932/icon-app-options.png) → _Configuration_ for the bottom Asset Publisher.
2. Under _Asset Selection_ choose _Content Set_.
3. Open _Select Content Set_ and click _Select_.
4. Click on the _Trending_ Content Set.

![Figure 2: Select the Content Set you want to use.](https://liferay-support.zendesk.com/hc/article\_attachments/360030191032/content-set-select-set.png)



1. Click _Save_.

Again, you can manage various display settings, but the items which appear and their order are determined by the Content Set criteria.

![Figure 3: You can see the results as the standard Asset Publisher output. You can create Widget Templates to add more style and pizzazz here.](https://liferay-support.zendesk.com/hc/article\_attachments/360030190992/content-set-dynamic-results.png)

### Adding Items to an existing Content Set <a href="#adding-items-to-an-existing-content-set" id="adding-items-to-an-existing-content-set"></a>

To demonstrate both the management of both static and dynamic Content Sets, upload a new image, tag it, and add it to the static set manually.

1. Upload a new image, and under _Categorization_ tag it as _trending_.
2. Without lifting another finger, the image is added to the top of the _Trending_ Content List.

![Figure 4: The result is dynamically added to the Content List wherever it is displayed.](https://liferay-support.zendesk.com/hc/article\_attachments/360030160111/content-set-dynamic-add.png)

To add it to the manual set, go back to _Site Administration_ → _Content & Data_ → _Site Builder_.

1. Click on _Space Program Images_ or select ![Options](https://liferay-support.zendesk.com/hc/article\_attachments/360030190952/icon-options.png) → _Edit_ next to _Space Program Images_.
2. Next to _Asset Entries_ click _Select_ → _Basic Document_.
3. Select the new image and click _Add_.
4. Navigate back to the _Home_ page to see your image added to the list.

Content Sets are a powerful feature which provide one place to easily define content and other assets to be displayed all over your site. Their reusability also means less repeated work involved in getting great content delivered to your users.
