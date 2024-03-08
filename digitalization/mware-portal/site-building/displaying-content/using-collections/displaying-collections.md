# Displaying Collections

You can display a collection by adding a collection page, or a collection display fragment. For more information about these display options, see [About Collections and Collection Pages](https://learn.liferay.com/web/guest/w/dxp/site-building/displaying-content/collections-and-collection-pages/about-collections-and-collection-pages#displaying-collections).

By default, any user can see a collection’s content. However, you can [personalize how users see this content using segments](https://learn.liferay.com/web/guest/w/dxp/site-building/displaying-content/collections-and-collection-pages/about-collections-and-collection-pages#customizing-collections).

NOTE

You must [create a collection](https://learn.liferay.com/web/guest/w/dxp/site-building/displaying-content/collections-and-collection-pages/creating-collections) before displaying it on a page.

### [Displaying Collections on a Page](https://learn.liferay.com/web/guest/w/dxp/site-building/displaying-content/collections-and-collection-pages/displaying-collections#displaying-collections-on-a-page) <a href="#displaying-collections-on-a-page" id="displaying-collections-on-a-page"></a>

The two easiest ways to display a collection are to add a collection page or to add a collection display fragment to an existing page.

#### [Displaying Collections on a Collection Page](https://learn.liferay.com/web/guest/w/dxp/site-building/displaying-content/collections-and-collection-pages/displaying-collections#displaying-collections-on-a-collection-page) <a href="#displaying-collections-on-a-collection-page" id="displaying-collections-on-a-collection-page"></a>

A collection page links a page to a collection. Collection pages include [collection display fragments](https://learn.liferay.com/web/guest/w/dxp/site-building/displaying-content/collections-and-collection-pages/displaying-collections#adding-a-collection-display-fragment-to-a-page) as part of the page.

Follow these steps to add a collection page:

1. Open the _Site Menu_ ( ![Site Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu.png) ) and go to _Site Builder_ → _Pages_.
2.  Click _New Page_ ( ![New Page](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png) ) and select _Collection Page_.

    ![Create a new collection page from the pages administration.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/collections-and-collection-pages/displaying-collections/images/01.png)
3. From the Collections tab, select one of your collections or click _Add_ ( ![Add Icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png) ) to [create a new collection](https://learn.liferay.com/web/guest/w/dxp/site-building/displaying-content/collections-and-collection-pages/creating-collections).
4. Select the _Master Page_ you want to use.
5. Enter the name of your collection page and click _Add_.
6. To show the collection, [map its content to page elements](https://learn.liferay.com/web/guest/w/dxp/site-building/displaying-content/collections-and-collection-pages/displaying-collections#mapping-components-to-display-a-collections-assets).

IMPORTANT

You cannot create a child page under a collection page.

You now have a collection page displaying your chosen collection, within a collection display fragment.

#### [Adding a Collection Display Fragment to a Page](https://learn.liferay.com/web/guest/w/dxp/site-building/displaying-content/collections-and-collection-pages/displaying-collections#adding-a-collection-display-fragment-to-a-page) <a href="#adding-a-collection-display-fragment-to-a-page" id="adding-a-collection-display-fragment-to-a-page"></a>

A collection display fragment shows a collection in a content page. It can be configured to display a collection in any style or layout.

![Use a collection display fragment to display a collection in the desired format.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/collections-and-collection-pages/displaying-collections/images/02.png)

To add an existing collection to a collection display fragment,

1. [Create a new page](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/adding-pages/adding-a-page-to-a-site) or [start editing a content page](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/using-content-pages/adding-elements-to-content-pages).
2. Click _Add_ ( ![Add widget icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add-widget.png) ) to open the Fragments and Widgets panel.
3. Under the Content Display category in the Fragments tab, drag the _Collection Display_ fragment to a place on the page.
4. Click on the _Collection Display_ fragment to show the configuration in the Selection panel.
5.  Under the General tab, click the _Collection_ field to map the selected collection to the fragment:

    ![Select a collection and map it to the collection display fragment.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/collections-and-collection-pages/displaying-collections/images/03.png)

The collection display fragment is now on the page and displaying the selected collection.

IMPORTANT

When adding a Collection Display Fragment to a page, only collections that can be mapped to editable fragments are available (Blog Entries, Documents, Knowledge Base Articles, Web Content Articles, and Custom Object Entries).

### [Configuring a Collection Display Fragment](https://learn.liferay.com/web/guest/w/dxp/site-building/displaying-content/collections-and-collection-pages/displaying-collections#configuring-a-collection-display-fragment) <a href="#configuring-a-collection-display-fragment" id="configuring-a-collection-display-fragment"></a>

Whether you added a collection display fragment manually or via a collection page, you can configure it by [editing the page](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/using-content-pages/adding-elements-to-content-pages) and selecting the fragment.

Selecting a collection display fragment divides it into sections for each asset in the collection. New options appear under the fragment’s general configuration after you’ve selected a collection:

**Style Display**: Choose the style used to format the assets in the list (e.g., grid or bulleted list styles). In versions earlier than 7.4 GA23 or U23, this configuration is called List Style.

**Layout**: Choose the number of columns in which to display the assets.

**Show Gutter**: Check the box to show the gutter (spacing) between columns. This option only appears when you choose the Grid style with a column layout (in versions 7.4 GA23+ or U23+).

**Show Empty Collection Alert**: Check the box to display an alert message when no results match the applied filters or the collection is empty.

**Empty Collection Alert**: If _Show Empty Collection Alert_ is enabled, choose the displayed alert message. This message is localizable.

**Vertical Alignment**: Choose how to align the items: top, middle, or bottom of each row. This option only appears when you choose the _Grid_ style (in versions 7.4 GA23+ or U23+).

**List item style**: Choose a style to format each asset. This option applies only to some list styles.

**Max number of items**: Select the maximum number of assets the fragment can display at a time.

![Click a collection display fragment with a configured collection to reveal more configuration options.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/collections-and-collection-pages/displaying-collections/images/04.png)

Publish the page when you’re ready for the assets in the collection to be displayed as configured.

TIP

In Liferay 7.4 GA23+ or U23+, if you have chosen the Grid style for a collection display fragment, you can configure the layout setting individually for each type of device. Click the device viewport buttons at the top of the screen to set this setting for each device. The tablet setting defaults to the same as the desktop’s setting, and the portrait and landscape phone settings default to 1 column.

#### [Mapping Components to Display a Collection’s Assets](https://learn.liferay.com/web/guest/w/dxp/site-building/displaying-content/collections-and-collection-pages/displaying-collections#mapping-components-to-display-a-collections-assets) <a href="#mapping-components-to-display-a-collections-assets" id="mapping-components-to-display-a-collections-assets"></a>

You can also configure the item style (how each asset is individually displayed) in the fragment by mapping display components to them.

Map these elements to the assets in the collection display fragment by dragging them into the cells of the fragment.

NOTE

Depending on the option you chose for a collection display fragment’s list style, you may not be able to drag components into the fragment. If you cannot drag components into the fragment, you can choose a different list style or select a different option under List Item Style.

1. In the page editor, click the _Collection Display_ representing your collection.
2. In the tools sidebar, click _Fragments and Widgets_ (![Fragment and Widgets](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add-widget.png)).
3.  From the Fragments and Widgets sidebar panel, drag and drop the components you want to use to show the collection items.

    ![Add fragments or widgets to the collection display and configure the display properties.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/collections-and-collection-pages/displaying-collections/images/05.gif)

    NOTE

    If the collection display contains more than one item, you can drag the fragment or widget to any of them. The rest of the items in your collection use the same layout settings automatically.
4. Double-click the _fragment_ or _widget_ on the collection display to access its mapping options.
5.  In the Mapping column, select the _field_ that you want to map to your collection.

    ![Map the fragments and widgets to the collection items](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/collections-and-collection-pages/displaying-collections/images/06.gif)
6.  Add more fragments or widgets as necessary and map the content to the content fields.

    ![Add fragments and widgets to the collection display and map the content fields.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/collections-and-collection-pages/displaying-collections/images/07.gif)
7. Click _Publish_.

### [Paginating Display Items](https://learn.liferay.com/web/guest/w/dxp/site-building/displaying-content/collections-and-collection-pages/displaying-collections#paginating-display-items) <a href="#paginating-display-items" id="paginating-display-items"></a>

7.4+

You can paginate the collection items using the pagination configuration. This option provides two important advantages:

* It is easier to show collections with a large number of items. Instead of showing all the items in the collection, you can decide how many items to show at once, while allowing users to move back and forward through the list of items in the collection.
* Breaking the visible list of items into smaller groups reduces the server overhead, improving the server response time and user experience.

NOTE

In Liferay 7.4 versions earlier than U18 and GA18, fewer pagination options are available. In these versions, you can still enable pagination and choose the maximum number of items to display at once, but you cannot change the number of pages shown in the pagination menu.

To paginate the display items in the collection display,

1. Click _Edit_ ( ![Edit icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-edit.png) ) on the content page with the collection display.
2. Click _Browser_ ( ![Browser icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-hierarchy.png) ) to open the Browser panel.
3. Under the Elements Browser, select the collection display fragment you want to paginate. The chosen fragment must be configured to display an existing collection.
4.  In the General Settings, choose the pagination options:

    **Pagination**: None (no pagination), Numeric, or Simple (Previous/Next controls).

    **Display All Pages**: Check the box to display the numbers or controls for every page in the pagination menu. If no pagination type is selected, this option becomes Display All Collection Items instead.

    **Maximum Number of Pages to Display**: The maximum number of pages shown in the pagination menu. This option only appears if the Display All Pages checkbox is not selected.

    **Maximum Number of Items per Page**: The maximum number of items from the collection to display per page, if pagination is enabled. If no pagination type is selected, this option becomes Maximum Number of Items to Display instead.

    ![Choose the pagination options for the collection display items.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/collections-and-collection-pages/displaying-collections/images/08.png)

If you enable pagination, the total number of pages available depends on both the total number of items in the collection and the setting for Maximum Number of Items per Page.

NOTE

By default, the collection display fragment displays up to 20 items at a time in edit mode. You can change this limit by navigating to _Control Panel_ → _Instance Settings_ → _Pages_ → _Page Editor_, and changing the number in the _Maximum Number of Items in Edit mode_ field. Displaying more than 20 items at once in edit mode, may negatively impact performance. This setting does not affect what users see on the page in View mode.

### [Viewing Collections Usage Throughout the Site](https://learn.liferay.com/web/guest/w/dxp/site-building/displaying-content/collections-and-collection-pages/displaying-collections#viewing-collections-usage-throughout-the-site) <a href="#viewing-collections-usage-throughout-the-site" id="viewing-collections-usage-throughout-the-site"></a>

You can find usage statistics for the collections on your site.

1. Open the _Site Menu_ ( ![Site Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu.png) ) and navigate to _Site Builder_ → _Collections_.
2. Under the Collections tab, click _Actions_ (![Actions Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) → _View Usages_ next to the Collection you want to review.

![Select View Usages to understand how your collections are being used through the site](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/collections-and-collection-pages/displaying-collections/images/09.png)

### [Liferay DXP 7.2](https://learn.liferay.com/web/guest/w/dxp/site-building/displaying-content/collections-and-collection-pages/displaying-collections#liferay-dxp-72) <a href="#liferay-dxp-72" id="liferay-dxp-72"></a>

#### [Displaying Content Sets](https://learn.liferay.com/web/guest/w/dxp/site-building/displaying-content/collections-and-collection-pages/displaying-collections#displaying-content-sets) <a href="#displaying-content-sets" id="displaying-content-sets"></a>

Content sets are primarily displayed through the Asset Publisher. It is currently the only method to display them out-of-the-box, but you can develop your own external applications or widgets to utilize content sets. In [Creating Content Sets](https://learn.liferay.com/web/guest/w/dxp/site-building/displaying-content/collections-and-collection-pages/creating-collections#creating-content-sets) you created two content sets. Now display them on a page.

[**Configuring the Asset Publisher for Content Sets**](https://learn.liferay.com/web/guest/w/dxp/site-building/displaying-content/collections-and-collection-pages/displaying-collections#configuring-the-asset-publisher-for-content-sets)

To display the content sets, start with a blank page, and then add the necessary asset publishers and configure them to display the content sets.

1. Create a new home page for your site as a widget page with a 1 column layout. If you’re using a fresh Liferay DXP bundle, you can just remove the Hello World widget from the sample Home page.
2. Open the _Add_ menu and add two _Content Management_ → _Asset Publishers_ to the page stacked vertically.
3. Click Options ( ![Options](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-app-options.png) ) → _Configuration_ for the top asset publisher.
4.  Under Asset Selection choose _Content Set_.

    ![The asset publisher has a number of options available for selecting its source for content.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/collections-and-collection-pages/displaying-collections/images/10.png)
5. Open the _Select Content Set_ and click _Select_.
6. Select the _Space Program Images_ content set.
7. Click _Save_.

Now the images appear at the top of the page. You can manage the way the content is displayed—like what metadata appears—or even create a widget template to style the content, but the displayed items and the order in which they appear are determined by the content set.

Now configure the bottom asset publisher with the other content set.

1. Click Options (![Options](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-app-options.png)) → _Configuration_ for the bottom asset publisher.
2. Under Asset Selection choose _Content Set_.
3. Open _Select Content Set_ and click _Select_.
4.  Click on the _Trending_ content set.

    ![Select the content set you want to use.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/collections-and-collection-pages/displaying-collections/images/11.png)
5. Click _Save_.

Again, you can manage various display settings, but the displayed items and their order are determined by the content set criteria.

![You can see the results as the standard asset publisher output. You can create widget templates to add more style and pizzazz here.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/collections-and-collection-pages/displaying-collections/images/12.png)

[**Adding Items to an Existing Content Set**](https://learn.liferay.com/web/guest/w/dxp/site-building/displaying-content/collections-and-collection-pages/displaying-collections#adding-items-to-an-existing-content-set)

To demonstrate the management of both static and dynamic content sets, upload a new image, tag it, and add it to the static set manually.

1. Upload a new image and, under Categorization, tag it as _trending_.
2.  The image is added to the top of the Trending content list.

    ![The result is added dynamically to the content list wherever it is displayed.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/collections-and-collection-pages/displaying-collections/images/13.png)
3. To add it to the manual set, go back to _Site Menu_ and navigate to _Content & Data_ → _Site Builder_.
4. Click on _Space Program Images_ or select ![Options](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-options.png) → _Edit_ next to Space Program Images.
5. Next to Asset Entries, click _Select_ → _Basic Document_.
6. Select the new image and click _Add_.
7. Navigate back to the home page to see your image added to the list.

Content sets provide one place to define content and other assets to be displayed all over your site. Their reusability also means less repeated work involved in getting great content delivered to your users.

\
