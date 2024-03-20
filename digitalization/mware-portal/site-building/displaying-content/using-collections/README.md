# Using Collections

Collections are sets of content items that you can use to group and filter information. Collections can include many different types of content or just one specific type of content. You can then display them on a collection page, which you can present in your desired format using page fragments.

![Use a collection page to quickly present a specific collection in your desired format.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/collections-and-collection-pages/about-collections-and-collection-pages/images/01.png)

Consider the following example. You plan to create a new section in your product web page with customer success stories. You want this section to show new success stories automatically as these stories are published. In this case, you can create a collection that groups all your success stories with a collection page that shows the collection’s content. In addition to collection pages, you can show this collection in other content pages using a Collection Display fragment (for more information, see Displaying Collections.)

There are two types of collections:

*   **Manual Collection**

    You select and maintain the items that are part of the collection manually. The items in a manual collection do not change automatically.

    For example, you create a manual collection named _Promotions_, which includes several promotional web content articles on your public website. After you select the items in the collection, you must add new promotional content manually to keep the collection up-to-date.
*   **Dynamic Collection**

    You define the type of items in the collection, along with criteria for these items. The collection’s items can change based on the criteria you define. A dynamic collection is useful when you want to make sure the content is up-to-date with new items matching the criteria.

    For example, for your Kitchenware Store, you create a collection including all blog posts with the _porcelain_ tag (this tag represents your criteria). The dynamic collection includes all the new blog posts with the _porcelain_ tag automatically.

![Use manual collections to choose the items in the collection yourself, or dynamic collections to automatically assemble the items based on your chosen criteria.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/collections-and-collection-pages/about-collections-and-collection-pages/images/02.png)

For information about how to create manual or dynamic collections, see Creating Collections.

### Displaying Collections <a href="#displaying-collections" id="displaying-collections"></a>

You can display collections using a collection page or a Collection Display fragment.

A collection page is a type of page linked to a collection. Collection pages ease the task of showing and customizing collections. For example, you can quickly add new items to your collection using a collection page.

![You can display your collection using a collection page](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/collections-and-collection-pages/about-collections-and-collection-pages/images/03.png)

A Collection Display fragment is a type of fragment that shows a collection. You can use this fragment to show your collection on any content page, page template, or display page.

![You can show the collection's content using a Collection Display fragment](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/collections-and-collection-pages/about-collections-and-collection-pages/images/04.png)

For more information, see Displaying Collections.

### Customizing Collections <a href="#customizing-collections" id="customizing-collections"></a>

You can associate a portal segment with your collection to display different collection items based on the segment configuration.

Consider the following example. You want to increase sales in your Kitchenware Store by offering exclusive promotions to registered users. You create a collection including promotions for registered users and news about your products. You want everyone to see the news about your products, but you want to restrict the promotions to registered users. You can create a new segment for registered users and link the segment to a new _Personalized Variation_ that filters the promotional content.

For information, read Personalizing Collections.

### Converting Asset Publisher Configurations to Collections <a href="#converting-asset-publisher-configurations-to-collections" id="converting-asset-publisher-configurations-to-collections"></a>

You can create collections directly or from an Asset Publisher widget. The Asset Publisher option is useful when you want to use your customization as a collection in other pages. For more information, read Creating Collections.

#### Managing Content Sets <a href="#managing-content-sets" id="managing-content-sets"></a>

A content set is exactly what it sounds like: a set of content items. In short, an administrator can defines a list of content, and then that list can be displayed. The way that the content set is displayed is determined by the method that is used to display it. For example, if the content set is being used by a smartwatch app, it could be displayed as a simple list of titles, and selecting a title would cause the full article to display on a connected mobile device. The same content set could be displayed in a web browser with the full content of each article.

NOTE

In previous versions you used the Asset Publisher to define and display either static lists of assets or dynamic lists based on criteria like tags, categories, or asset type. Content sets take the core idea of defining different types of asset lists and expand on it. Content lists are created outside of the context of a specific application or widget and can be used and re-used across different channels and applications.

#### Creating and Displaying Content Sets <a href="#creating-and-displaying-content-sets" id="creating-and-displaying-content-sets"></a>

Content sets are created through the Site Administration interface. All the features for creating and managing content sets are contained here. They are displayed using  widgets or your own custom applications. Read our guides for information on Creating Content Sets and Displaying Content Sets.

#### Content Set Personalization <a href="#content-set-personalization" id="content-set-personalization"></a>

Content sets can have variations driven by the portal’s personalization engine. After you create a content set, if you have at least one user segment created, you can create a personalized experience of the content set for that segment. To learn to harness the power of experience personalization for content sets, see Content Set Personalization.

#### Converting Asset Publisher Configurations to Content Sets <a href="#converting-asset-publisher-configurations-to-content-sets" id="converting-asset-publisher-configurations-to-content-sets"></a>

You may have already gone through a great deal of work to create a perfect, curated list of content through the Asset Publisher, but now you want to display that list elsewhere without duplicating your work. You can do that with content sets. Read the Converting Asset Publisher Configuration to Content Sets article to learn more.

\
