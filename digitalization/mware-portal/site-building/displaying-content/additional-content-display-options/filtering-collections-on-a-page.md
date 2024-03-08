# Filtering Collections on a Page

To improve the experience for Users viewing Collections on your Site, you can add Page Fragments to filter content in those Collections. For example, you can add a Fragment that can be used to search for and filter displayed Collections content using keywords.

There are two Page Fragments you can add to your pages to help Users filter content in displayed Collections: the [Collection Filter Fragment](https://learn.liferay.com/web/guest/w/dxp/site-building/displaying-content/additional-content-display-options/filtering-collections-on-a-page#using-the-collection-filter-fragment) and the [Applied Filters Fragment](https://learn.liferay.com/web/guest/w/dxp/site-building/displaying-content/additional-content-display-options/filtering-collections-on-a-page#using-the-applied-filters-fragment).

### [Using the Collection Filter Fragment](https://learn.liferay.com/web/guest/w/dxp/site-building/displaying-content/additional-content-display-options/filtering-collections-on-a-page#using-the-collection-filter-fragment) <a href="#using-the-collection-filter-fragment" id="using-the-collection-filter-fragment"></a>

The Collection Filter Fragment is used to filter the content displayed in a chosen Collection on the page. Users can search for keywords, or select categories and tags to filter the Collection associated with the filter Fragment.

Follow these steps to add and configure a Collection Filter Fragment to a page (that is already displaying a Collection in a [Collection Display Fragment](https://learn.liferay.com/web/guest/w/dxp/site-building/displaying-content/collections-and-collection-pages/displaying-collections#configuring-a-collection-display-fragment)):

1. While editing the page, add a Collection Filter Fragment anywhere on the page.
2.  In the Fragment’s _General_ configuration, select the desired Collection. The Collection must be displayed in Collection Display Fragments on the page for it to have an effect.

    ![Configure the Fragment to use a Collection that is already displayed in a Collection Display Fragment.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/additional-content-display-options/filtering-collections-on-a-page/images/01.png)

    The _Filter_ drop-down menu appears when you have selected a compatible Collection.
3.  Use the Filter menu to choose _Keywords_, _Category_, or _tags_ filters.

    NOTE

    If modules have been deployed that add custom Collection filters (by implementing the [`FragmentCollectionFilter`](https://github.com/liferay/liferay-portal/blob/master/modules/apps/fragment/fragment-collection-filter-api/src/main/java/com/liferay/fragment/collection/filter/FragmentCollectionFilter.java) and [`InfoFilter`](https://github.com/liferay/liferay-portal/blob/master/modules/apps/info/info-api/src/main/java/com/liferay/info/filter/InfoFilter.java) interfaces), you can also choose these filters from the list.

The Collection Filter fragment’s behavior changes depending on whether you configured the Keywords, Category, or Tags filter type.

```log
!!! note "note"
    Collection Filter fragments used in Collection Display fragments only work if mapped to object collection providers or blueprint collection providers.
```

#### [Filtering by Keyword](https://learn.liferay.com/web/guest/w/dxp/site-building/displaying-content/additional-content-display-options/filtering-collections-on-a-page#filtering-by-keyword) <a href="#filtering-by-keyword" id="filtering-by-keyword"></a>

When Keywords is selected, the Collection Filter fragment appears as a search bar, where users can filter the displayed content of collections by entering keywords.

![The Keywords filter type presents a search bar to users to enter keywords to filter by.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/additional-content-display-options/filtering-collections-on-a-page/images/03.png)

#### [Filtering by Category](https://learn.liferay.com/web/guest/w/dxp/site-building/displaying-content/additional-content-display-options/filtering-collections-on-a-page#filtering-by-category) <a href="#filtering-by-category" id="filtering-by-category"></a>

When Category is selected, the Source input field can be used to configure which categories users can select.

![Select the vocabulary or category that contains the desired category selection.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/additional-content-display-options/filtering-collections-on-a-page/images/02.png)

1. Click _Select This Level_ when you have selected the vocabulary or parent category containing the desired categories.
2. Publish the page.

After configuring the Category filter, the Collection Filter fragment displays a menu of categories. Users can select one or more categories to filter the collection’s displayed content.

![The Category filter type presents a list of categories for Users to enable and apply as a filter.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/additional-content-display-options/filtering-collections-on-a-page/images/04.png)

#### [Filtering by Tags](https://learn.liferay.com/web/guest/w/dxp/site-building/displaying-content/additional-content-display-options/filtering-collections-on-a-page#filtering-by-tags) <a href="#filtering-by-tags" id="filtering-by-tags"></a>

When Tags is selected, users can type in the provided field and see a drop-down showing existing tags that can be selected.

![The Tags filter type presents a search bar with autocomplete that displays available tags.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/additional-content-display-options/filtering-collections-on-a-page/images/06.png)

### [Using the Applied Filters Fragment](https://learn.liferay.com/web/guest/w/dxp/site-building/displaying-content/additional-content-display-options/filtering-collections-on-a-page#using-the-applied-filters-fragment) <a href="#using-the-applied-filters-fragment" id="using-the-applied-filters-fragment"></a>

The Applied Filters Fragment is used to see a list of filters applied with the [Collection Filter Fragment](https://learn.liferay.com/web/guest/w/dxp/site-building/displaying-content/additional-content-display-options/filtering-collections-on-a-page#using-the-collection-filter-fragment).

Follow these steps to add an Applied Filters Fragment to a page (that already has a configured Collection Filter Fragment):

1. While editing the page, add an Applied Filter Fragment anywhere on the page.
2. In the Applied Filter Fragment’s _General_ configuration, select the desired Collection. The same Collection must be both displayed on the page and configured with a [Collection Filter Fragment](https://learn.liferay.com/web/guest/w/dxp/site-building/displaying-content/additional-content-display-options/filtering-collections-on-a-page#using-the-collection-filter-fragment).
3. Publish the page.

Now, when the User filters content with the Collection Filter Fragment, the Applied Filters Fragment shows a list of the filters currently applying to the configured Collection. Users can also click the _X_ button on each filter listed to remove the filter.

![The Applied Filters Fragment shows all filters currently applied to the configured Collection.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/additional-content-display-options/filtering-collections-on-a-page/images/05.png)

\
