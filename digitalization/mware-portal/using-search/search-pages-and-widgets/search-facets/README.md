# Search Facets

## [Facets](https://learn.liferay.com/web/guest/w/dxp/using-search/search-pages-and-widgets/search-facets#facets) <a href="#facets" id="facets"></a>

Enter a keyword in the Search Bar and click the Search button. The default search experience redirects to a page with results on the right and a collection of _facets_ on the left.

* [Site Facet](https://learn.liferay.com/web/guest/w/dxp/using-search/search-pages-and-widgets/search-facets/site-facet)
* [Type Facet](https://learn.liferay.com/web/guest/w/dxp/using-search/search-pages-and-widgets/search-facets/type-facet)
* [Category Facet](https://learn.liferay.com/web/guest/w/dxp/using-search/search-pages-and-widgets/search-facets/category-facet)
* [Tag Facet](https://learn.liferay.com/web/guest/w/dxp/using-search/search-pages-and-widgets/search-facets/tag-facet)
* [Folder Facet](https://learn.liferay.com/web/guest/w/dxp/using-search/search-pages-and-widgets/search-facets/folder-facet)
* [User Facet](https://learn.liferay.com/web/guest/w/dxp/using-search/search-pages-and-widgets/search-facets/user-facet)
* [Modified Facet](https://learn.liferay.com/web/guest/w/dxp/using-search/search-pages-and-widgets/search-facets/modified-facet)
* [Custom Facet](https://learn.liferay.com/web/guest/w/dxp/using-search/search-pages-and-widgets/search-facets/custom-facet)

![Example page of search results.](https://resources.learn.liferay.com/images/dxp/latest/en/using-search/search-pages-and-widgets/search-facets/images/01.png)

A facet aggregates search results by a common characteristic, or facet term.

### [Using Facets](https://learn.liferay.com/web/guest/w/dxp/using-search/search-pages-and-widgets/search-facets#using-facets) <a href="#using-facets" id="using-facets"></a>

Click one or more facet terms to filter through the search results efficiently. For example, if you search for documents related to Apollo, you can check a term in the Type facet.

![Apollo search results filtered by type.](https://resources.learn.liferay.com/images/dxp/latest/en/using-search/search-pages-and-widgets/search-facets/images/02.png)

To look specifically for documents related to Apollo 11, you might also check the term in the folder facet.

![Apollo search results filtered by folder.](https://resources.learn.liferay.com/images/dxp/latest/en/using-search/search-pages-and-widgets/search-facets/images/03.png)

In this way, you can pare down your search results.

### [Multiple Facet Selection](https://learn.liferay.com/web/guest/w/dxp/using-search/search-pages-and-widgets/search-facets#multiple-facet-selection) <a href="#multiple-facet-selection" id="multiple-facet-selection"></a>

As seen in the example above, selecting terms in different facets is subtractive (i.e., it uses the AND operator). That is, only results that match all of the filter criteria are returned.

However, selecting terms within an individual facet is additive (i.e., it uses the OR operator). That is, the combined results for each term are returned. For example, if you wanted to filter the search results for both the Apollo 11 folder and Apollo 14 folder, you could check both terms.

![Apollo search results for both folders.](https://resources.learn.liferay.com/images/dxp/latest/en/using-search/search-pages-and-widgets/search-facets/images/04.png)

### [Facets and Friendly URLs](https://learn.liferay.com/web/guest/w/dxp/using-search/search-pages-and-widgets/search-facets#facets-and-friendly-urls) <a href="#facets-and-friendly-urls" id="facets-and-friendly-urls"></a>

The Search functionality uses friendly search URLs for facet filtering. With default settings, here’s the default main search URL when searching for keyword _test_:

```log
http://localhost:8080/web/guest/search?q=test
```

Selecting a facet term adds a new parameter to the above URL. For example, selecting Blogs Entry from the Type facet results in this URL:

```log
http://localhost:8080/web/guest/search?q=test&type=com.liferay.blogs.model.BlogsEntry
```

Selecting another facet term from the same facet category appends the same parameter again, but with the newly selected value:

```log
http://localhost:8080/web/guest/search?q=test&type=com.liferay.blogs.model.BlogsEntry&type=com.liferay.portal.kernel.model.User
```

The rest of the facets work the same way. Filtering by the last hour option in the Last Modified facet produces this URL:

```log
http://localhost:8080/web/guest/search?q=test&modified=past-hour
```

The parameter names are configurable for each facet.

To learn more details of each facet type, refer to their individual articles.

\
