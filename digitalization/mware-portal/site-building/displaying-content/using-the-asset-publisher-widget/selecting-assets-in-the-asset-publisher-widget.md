# Selecting Assets in the Asset Publisher Widget

You can configure the Asset Publisher widget to select assets dynamically based on specific criteria, or manually, specifying exactly what assets to display. You can also display assets from collections.

NOTE

On version 7.2 and below, collections are referred to as content sets.

1. Create a new page or edit an existing one
2. While editing the page, look at the Fragments and Widgets tab on the left of the page editor and search for the _Asset Publisher_ widget.
3. Drag and drop the _Asset Publisher_ widget into the editing area.
4.  Click _Actions_ (![Actions icon.](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) at the top-right corner of the Asset Publisher widget.

    ![Click Actions to access the Asset Publisher configuration options.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/using-the-asset-publisher-widget/selecting-assets-in-the-asset-publisher-widget/images/01.png)
5.  Select _Configuration_ to display the Configuration dialog.

    Under Asset Selection, select the assets you want to show in your widget.

    Under Display Settings, configure how to display your content.

    Under Subscriptions, configure email subscriptions and RSS feed subscriptions.

    ![The configuration window has different Asset Selection options under the Setup tab.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/using-the-asset-publisher-widget/selecting-assets-in-the-asset-publisher-widget/images/02.png)
6. Once you select your assets and configure the widget, click _Save_.

### Asset Selection Options <a href="#asset-selection-options" id="asset-selection-options"></a>

Assets are selected for display in the Asset Publisher widget using one of these selection methods:

* Collection
* Dynamic
* Manual

#### Collection <a href="#collection" id="collection"></a>

Select this option to choose from your previously saved collections or collection providers. To learn more about collections, see About Collections and Collection Pages.

1. Under Select Collection, click _Select_.
2. Select either the _Collections_ or _Collections Providers_ tab
3. Choose one of your existing collections or collection providers.
4. Click _Save_.

![Click Select and choose from your saved collections.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/using-the-asset-publisher-widget/selecting-assets-in-the-asset-publisher-widget/images/03.png)

NOTE

Developers can use collection providers to create specific collections with more advanced criteria. To learn more, read the information about Creating an Information List Provider in the Info Framework developer documentation.

#### Dynamic <a href="#dynamic" id="dynamic"></a>

Using the Dynamic asset selection, you can include assets in the Asset Publisher widget automatically, based on different criteria.

![Define the options for the dynamic asset collection, including Source, Scope, Filter, Custom User Attribute, and Ordering.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/using-the-asset-publisher-widget/selecting-assets-in-the-asset-publisher-widget/images/04.png)

1. In the Scope section, choose to display assets from an individual site or other sites.
2.  In the Source section, use the Asset Type drop-down menu to select your assets. Select a specific asset type or _Select More Than One_ to choose from different types.

    With some options (e.g. Web Content Article and Document) you may need to select a subtype (e.g. Web Content Article Structures and Document Type). You can also choose any subtype or select more than one subtype.
3.  In the Filter section, define the rules for the assets in your Asset Publisher widget. You can define rules based on categories, tags, or keywords.

    You can choose filters for items such as Contain/Do Not Contain All or Any of the following Categories, Keywords, or Tags and, then specify the categories, keywords, or tags.
4.  Include a Custom User Attribute if you have defined one.

    The assets only appear if they match the included custom user profile attributes.
5.  In the Ordering section, choose the presentation order of assets in the Asset Publisher widget.

    Select a primary and a secondary field to order the assets by. You can also choose to order the assets in ascending or descending order.

    WARNING

    When selecting web content articles as the asset type and ordering by a localized web content structure field, all assets’ field values should be localized when other languages are applied to the site. Only translated assets are ordered when another language is applied.
6. (Optional) Click _Create a collection from this configuration_, enter a title, and click _Save_ to save the configuration as a new collection.

#### Manual <a href="#manual" id="manual"></a>

Here you can set parameters and manually filter specific assets:

1. In the Scope section, choose to display assets from an individual site or other sites.
2. In the Asset Entries section, use the Select button to filter for specific types of assets.
3. For the type of asset you select, check the assets you want to include.
4.  Click _Add_.

    ![Here is an example of filtering for basic web content and manually selecting items](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/using-the-asset-publisher-widget/selecting-assets-in-the-asset-publisher-widget/images/05.png)
5. (Optional) Click _Create a collection from this configuration_, enter a title, and click _Save_ to save the configuration as a new collection.

\
