# Creating Collections

You can create _Manual_ or _Dynamic_ Collections. For information about the different types of Collections, see About Collections and Collection Pages.

### Creating a Manual Collection <a href="#creating-a-manual-collection" id="creating-a-manual-collection"></a>

1. Go to _Site Administration_ → _Site Builder_ → _Collections_.
2. Click on the _New_ button (![Add](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png)) and select _Manual Collection_.
3. Add a _Title_ for your Manual Collection and click _Save_.
4. In the _Item Type_ drop-down menu, select the item type you want to include in the Collection.
   *   If you want to include only one item type

       1. Select the type under _Single Item Type_.
       2. Optionally, if your _Item Type_ includes a subtype, select this in the _Item Subtype_ drop-down menu.
       3. Click _Save_.

       ![Configure the content Type and Subtype for the Manual Collection](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/collections-and-collection-pages/creating-collections/images/01.png)
   *   If you want to include more than one item type

       1. Select _Select More Than One_ under _Multiple Item Types_.
       2. In the dual list box, add or remove the item types you want to include in your collection.

       ![Select the different item Types for the Manual Collection](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/collections-and-collection-pages/creating-collections/images/02.png)
5. Click _Save_.
6.  Click _Select_ next to _Collection Items_ and select one of your item types.

    ![Select the items you want to include in the Manual Collection](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/collections-and-collection-pages/creating-collections/images/03.png)
7. In the selection dialog, check the items you want to include.
8. Click _Add_.
9. If your Manual Collection includes more than one item type, repeat steps 6 to 8 for the other types.

Note that Documents and Media folders and Web Content folders can be added to a Collection.

NOTE

While Documents and Media and Web Content folders can be added to a collection, the content is not displayed when the folder is published on a Collections Page or in an Asset Publisher widget. Even if these assets are added to a collection, they can only be viewed separately. This is a known limitation and will be fixed in a future release.

### Creating a Dynamic Collection <a href="#creating-a-dynamic-collection" id="creating-a-dynamic-collection"></a>

1. Go to _Site Administration_ → _Site Builder_ → _Collections_.
2. Click on the _New_ button (![Add](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png)) and select _Dynamic Collection_.
3. Add a _Title_ for your Manual Collection and click _Save_.
4. In the _Item Type_ drop-down menu, select the item type you want to include in the Collection.
   *   If you want to include only one item type,

       1. Select the type under _Single Item Type_.
       2. Optionally, if your _Item Type_ includes a subtype, select this in the _Item Subtype_ drop-down menu.
       3. To filter you _Item Subtype_ by a specific field, enable the _Filter by Field_ switch, click _Select_, choose the field, and click _Apply_.
       4. Click _Save_.

       ![You can filter the Item Subtype in your Collection by one of the item fields](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/collections-and-collection-pages/creating-collections/images/09.gif)
   *   If you want to include more than one item type,

       1. Select _Select Types_ under _Multiple Item Types_.
       2. In the dual list box, add or remove the item types you want to include in your collection.

       ![Select the different item Types for the Manual Collection](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/collections-and-collection-pages/creating-collections/images/02.png)
5.  Configure the criteria for your Dynamic Collection, based on _Scope_, _Filter_, _Content Recommendation_, or _Ordering_.

    **Scope:** Define the source of items in your Dynamic Collection. By default, the source is the current site.

    **Filter:** Configure the rules for the items in the Dynamic Collection. For example, you may want to include only items with the “promotion” tag.

    ![Filter your Dynamic Collection by adding one or more rules.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/collections-and-collection-pages/creating-collections/images/10.png)

    TIP

    You can add multiple rules to your Filter clicking on the Add button. The final content in the Dynamic Collection is the result of adding all the rules.

    **Content Recommendation:** Enable this option to display content based on user behavior. For more information, see Configuring Content Recommendations.

    NOTE

    The Content Recommendation option is available when you connect the DXP instance to Analytics Cloud and synchronize the content.

    **Ordering:** Items in the collection appear using the order criteria you define here.
6. Click _Save_.

### Creating a Collection from an Asset Publisher <a href="#creating-a-collection-from-an-asset-publisher" id="creating-a-collection-from-an-asset-publisher"></a>

You can create a new Collection from an Asset Publisher Widget. This is useful when you have an Asset Publisher customization that you want to use as a Collection in other pages.

![You can transform an Asset Publisher widget into a Collection.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/collections-and-collection-pages/creating-collections/images/06.png)

1. Go to _Site Administration_ → _Site Builder_ → _Pages_.
2.  Click the _Actions_ button (![Add](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) next to the page that contains the Asset Publisher and select _Edit_.

    ![Edit the page which contains your Asset Publisher.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/collections-and-collection-pages/creating-collections/images/11.png)
3.  Hover over the _Asset Publisher_, click the _Options_ icon (![Options](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-app-options.png)) in the widget’s menu, and select _Configuration_.

    ![Configure the Asset Publisher Widget.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/collections-and-collection-pages/creating-collections/images/07.png)
4. In the _Asset Publisher - Configuration_ dialog, click the _Setup_ tab and _Asset Selection_ sub-tab.
5.  Scroll down and click _Create a Collection from this Configuration_.

    ![Click the Create a collection from this configuration to transform the Asset Publisher widget into a new Collection.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/collections-and-collection-pages/creating-collections/images/08.png)
6. Enter the _Title_ for your Collection and click _Save_.
7. Close the _Asset Publisher - Configuration_ dialog.
8. Find your new Collection in _Site Administration_ → _Site Builder_ → _Collections_.

#### Creating Content Sets <a href="#creating-content-sets" id="creating-content-sets"></a>

You can create Content Sets through the Content Sets interface in Site Administration. Content Sets can use either Manual or Dynamic selection. You can create any number of Content Sets and display them through the Asset Publisher or custom applications. Content Sets can also have personalized variations that provide different experiences for different users based on criteria that you specify. Criteria management is shared with the Asset Publisher. For more information on each option, see Selecting Assets in the Asset Publisher.

**Creating a Manual Content Set**

To demonstrate the creation of a Manual Content Set, create a Content Set that contains a number of images to be displayed on the Frontpage of the fictitious Space Program website. To prepare for this exercise, upload some appropriate images to _Documents and Media_ to use for the Content Set.

1.  Go to _Site Administration_ → _Content & Data_ and select _Content Sets_.

    ![Content Sets is found in the Content & Data section of Site Administration.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/collections-and-collection-pages/creating-collections/images/20.png)
2. Click ![Add](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png) and select _Manual Selection_.
3. Name your Content Set _Space Program Images_.

On the next screen, you can select the assets to include in the Content Set.

1.  Click _Select_ → _Basic Document_.

    ![You can select the type of asset to add to the Content Set.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/collections-and-collection-pages/creating-collections/images/21.png)
2. Now, check the boxes for each image that you want to add and click _Add_.

Now this Content Set can be displayed anywhere on the site where it was created. You can add or remove items from the set, and it will automatically update it wherever it is displayed.

**Creating a Dynamic Content Set**

To demonstrate the creation of a Dynamic Content Set, create a Content Set that contains a number of varied assets that are tagged as “trending.” In order for this to work, you will need some number of existing assets with the appropriate tag.

1. From the _Content Sets_ page, click ![Add](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png) → _Dynamic Selection_.
2. Enter _Trending_ for the name and click _Save_.

With Dynamic Content Sets, you can choose the _Source_, _Scope_, _Filter_, and _Ordering_ for the items in the set.

1. Leave _Source_ as _Any_ and _Scope_ as _Current Site_
2.  Open _Filter_, make sure it is set to _Contains Any of the following Tags_, and then enter “trending” in the _Tags_ box.

    ![Content Sets use the same filter system as the Asset Publisher.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/collections-and-collection-pages/creating-collections/images/22.png)
3. Open _Ordering_ and set it to _Order By_: _Publish Date_, _And Then By_: _Title_.
4. Click _Save_.

This will create a Content Set which will contain any items that are currently tagged as _trending_ and any future items with the _trending_ tag will be added to the Content Set automatically.

Now that you have your Content Sets created, you can display them on a page.

#### Converting Asset Publisher Configurations to Content Sets <a href="#converting-asset-publisher-configurations-to-content-sets" id="converting-asset-publisher-configurations-to-content-sets"></a>

In the previous two guides in this section, you’ve seen Creating Content Sets and Displaying Content Sets demonstrated. Next, try out converting an existing Asset Publisher configuration to a Content Set.

In this case, you have an Asset Publisher on a page, which is configured to display images tagged as _trending_ in reverse alphabetical order by title. This might not be too hard to reproduce in the _Content Set_ creator, but it’s even easier to create the Content Set definition directly from the Asset Publisher.

1. Go to ![Options](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-app-options.png) → _Configuration_ for the Asset Publisher.
2.  Click _Create a content set from this configuration_.

    ![You can generate a Content Set directly from the Asset Publisher configuration.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/collections-and-collection-pages/creating-collections/images/23.png)
3. Enter the title and click _Save_.

And as quickly as that you have a new Content Set that you can use with Asset Publishers anywhere on the site.

![The Content Set is added right alongside any existing sets.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/collections-and-collection-pages/creating-collections/images/24.png)

Great! You converted your Asset Publisher configuration to a Content Set.

\
