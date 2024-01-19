# Display Settings

The Search Results widget’s default display is a paginated list. Each list item is a summarized hit to a search query. Click on a specific result to look at it in more detail. Configure display options by opening the Search Results options menu (![Options](https://liferay-support.zendesk.com/hc/article\_attachments/360030192972/icon-app-options.png)) and selecting _Configuration_.

**Enable Highlighting**

Highlight the search terms where they appear in the search result’s title or summary.

**Display Selected Result in Context**

When an asset is clicked, show it in its native application. For example, if you click on a blog post in the search results, you see where the Blogs Entry is posted in the Blogs application. Note that you’re not in the search context after clicking on a search result. When this option is unchecked, the asset displays in an Asset Publisher window while still in the search context. If you have the right permissions, you can even edit the content directly from the Search context. Click the back arrow to return to the search results.

**Display Results in Document Form**

Display results as search documents. Never use this in production. Developers use this feature to view search responses in their indexed, document-based format. Part of a developer’s job when writing search indexers is to convert documents (the objects that get indexed) to the actual object and back again. Thus, developers can see how their objects are being indexed. Once enabled, click the _Details…_ link below the result summary to expand the result’s document view.

![Figure 1: Viewing a results document lets you inspect exactly whats being indexed for a particular asset. This is just a small portion of one document.](https://liferay-support.zendesk.com/hc/article\_attachments/360030192952/search-results-document.png)

The next three configurations concern results pagination.

![Figure 2: The number of results per page and the URL parameter names used to control pagination behavior are configurable.](https://liferay-support.zendesk.com/hc/article\_attachments/360030192992/search-results-pagination.png)

**Pagination Start Parameter Name**

Set the name of the URL parameter for the results page. If the default value _start_ is preserved, this URL displays when the User navigates to the second results page after searching for _test_:

```
  http://localhost:8080/web/guest/search?q=test&start=2
```

**Pagination Delta**

Set the number of results to display per results page. Defaults to _20_ unless you customized the `search.container.page.default.delta` property in your `portal-ext.properties` file.

**Pagination Delta Parameter Name**

Set the name of the URL parameter that stores the Pagination Delta value. This becomes visible in the browser if the User changes the number. If the User selects 10 results per page and searches for _test_, the Search Page is reloaded with this URL:

```
  http://localhost:8080/web/guest/search?q=test&delta=10
```

**Federated Search Key**

If this widget is participating in a search on a non-default index, enter the key of the alternate search index. If not set, the widget participates in the default search, against the default index (portal`-[comanyId]`. This value is usually the name of an application-defined index.

**Fields to Display**

If searching an alternate index using the Federated Search Key configuration, specify what fields to search from that index.

For further reading, check out how to return suggestions for better search terms (for example, “Did you mean…”) when not enough results are returned initially.
