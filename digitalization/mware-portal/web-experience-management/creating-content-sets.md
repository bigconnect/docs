# Creating Content Sets

Content Sets are created by content administrators through the Content Sets interface in Site Administration. Content Sets can use either Manual or Dynamic selection, and you can create any number of Content Sets, and display them through the Asset Publisher or custom applications. Content Sets can also have personalized variations which provide different experiences for different users based on criteria that you specify.

### Creating a Manual Content Set <a href="#creating-a-manual-content-set" id="creating-a-manual-content-set"></a>

To demonstrate the creation of a Manual Content Set, create a Content Set that contains a number of images to be displayed on the Frontpage of the fictitious Space Program website. To prepare for this exercise, upload some appropriate images to _Documents and Media_ to use for the Content Set.

1.  Go to _Site Administration_ → _Content & Data_ and select _Content Sets_.

    <img src="https://liferay-support.zendesk.com/hc/article_attachments/360030190872/content-sets-empty-page.png" alt="Figure 1: Content Sets is found in the Content &#x26; Data section of Site Administration." data-size="original">

    Figure 1: Content Sets is found in the Content & Data section of Site Administration.
2. Click ![Add](https://liferay-support.zendesk.com/hc/article\_attachments/360030160091/icon-add.png) and select _Manual Selection_.
3. Name your Content Set _Space Program Images_.

On the next screen, you can select the assets to include in the Content Set.

1.  Click _Select_ → _Basic Document_.

    <img src="https://liferay-support.zendesk.com/hc/article_attachments/360030190892/content-sets-select-document.png" alt="Figure 2: You can select the type of asset to add to the Content Set." data-size="original">

    Figure 2: You can select the type of asset to add to the Content Set.
2. Now, check the boxes for each image that you want to add and click _Add_.

Now this Content Set can be displayed anywhere on the site where it was created. You can add or remove items from the set, and it will automatically update it whereever it is displayed.

### Creating a Dynamic Content Set <a href="#creating-a-dynamic-content-set" id="creating-a-dynamic-content-set"></a>

To demonstrate the creation of a Dynamic Content Set, create a Content Set that contains a number of varied assets that are tagged as “trending.” In order for this to work, you will need some number of existing assets with the appropriate tag.

1. From the _Content Sets_ page, click ![Add](https://liferay-support.zendesk.com/hc/article\_attachments/360030160091/icon-add.png) → _Dynamic Selection_.
2. Enter _Trending_ for the name and click _Save_.

With Dynamic Content Sets, you can choose the _Source_, _Scope_, _Filter_, and _Ordering_ for the items in the set.

1. Leave _Source_ as _Any_ and _Scope_ as _Current Site_
2.  Open _Filter_, make sure it is set to _Contains Any of the following Tags_, and then enter “trending” in the _Tags_ box.

    <img src="https://liferay-support.zendesk.com/hc/article_attachments/360030190852/content-set-trending-filter.png" alt="Figure 3: Content Sets use the same filter system as the Asset Publisher." data-size="original">

    Figure 3: Content Sets use the same filter system as the Asset Publisher.
3. Open _Ordering_ and set it to _Order By_: _Publish Date_, _And Then By_: _Title_.
4. Click _Save_.

This will create a Content Set which will contain any items that are currently tagged as _trending_ and any future items with the _trending_ tag will be added to the Content Set automatically.

Now that you have your Content Sets created, you can display them on a page.
