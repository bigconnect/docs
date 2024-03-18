# Configuring the Search Results Widget

Configure the Search Results widget by opening its options menu (![Options](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-app-options.png)) and selecting _Configuration_.

![The display of Search Results is configured in the widget's configuration screen.](https://resources.learn.liferay.com/images/dxp/latest/en/using-search/search-pages-and-widgets/search-results/configuring-the-search-results-widget/images/02.png)

Settings can be conceptually divided into display, pagination, and federated search settings. In addition, during development and testing, you can inspect the search engine document for each result by checking _Display Results in Document Form_.

### [Displaying Search Results](https://learn.liferay.com/web/guest/w/dxp/using-search/search-pages-and-widgets/search-results/configuring-the-search-results-widget#displaying-search-results) <a href="#displaying-search-results" id="displaying-search-results"></a>

**Display Template:** Select the [Widget Template](https://learn.liferay.com/web/guest/w/dxp/site-building/displaying-content/additional-content-display-options/styling-widgets-with-widget-templates) that styles the returned search results. The default display style is a paginated list. Each list item is a summarized hit to a search query. Click the _Manage Templates_ link to add a new Widget Template for the Search Results widget.

![The Card layout is an alternative to the default search experience.](https://resources.learn.liferay.com/images/dxp/latest/en/using-search/search-pages-and-widgets/search-results/configuring-the-search-results-widget/images/01.png)

**Show Default Empty Result Message** (7.4 U82+): Display the default message when there are no search results. Only disable this if using a custom display template that sets an empty results message.

**Show Default Pagination** (7.4 U82+): Use the default pagination. Only disable this if using a custom display template that defines pagination.

**Enable Highlighting:** Highlight the search terms where they appear in the search result’s title or summary.

![With highlighting enabled, matches to the keyword are highlighted in the result summary.](https://resources.learn.liferay.com/images/dxp/latest/en/using-search/search-pages-and-widgets/search-results/configuring-the-search-results-widget/images/03.png)

**Display Selected Result in Context:** Show clicked assets in their native display widgets, if one is detected on a page in the same Site. For example, if you click on a blog post in the search results, you see the Blogs Entry in the Blogs application. You’re no longer in the search context after clicking on a search result.

When this option is unchecked or an asset is not displayed on a page in the Site, the asset instead displays in an Asset Publisher window while still in the search page context. If you have the right permissions, you can even edit the content directly from the Search context. Click the back arrow to return to the search results.

![If the display in context option is unchecked or the content isn't displayed on a Page in the Site, an Asset Publisher widget displays the content in the Search Page itself.](https://resources.learn.liferay.com/images/dxp/latest/en/using-search/search-pages-and-widgets/search-results/configuring-the-search-results-widget/images/04.png)

### [Configuring Results Pagination](https://learn.liferay.com/web/guest/w/dxp/using-search/search-pages-and-widgets/search-results/configuring-the-search-results-widget#configuring-results-pagination) <a href="#configuring-results-pagination" id="configuring-results-pagination"></a>

Results pagination appears at the bottom of the search results:

![The number of results per page and the URL parameter names used to control pagination behavior are configurable.](https://resources.learn.liferay.com/images/dxp/latest/en/using-search/search-pages-and-widgets/search-results/configuring-the-search-results-widget/images/06.png)

**Pagination Start Parameter Name:** Set the name of the URL parameter for the results page. If the default value _start_ is preserved (and the search is for _test_), you can see the parameter when the User navigates to the second results page:

```log
http://localhost:8080/web/guest/search?q=test&start=2
```

**Pagination Delta:** Set the number of results to display per results page. Defaults to _20_ unless you customized the `search.container.page.default.delta` [property](https://learn.liferay.com/reference/latest/en/dxp/propertiesdoc/portal.properties.html#Search%20Container) in your `portal-ext.properties` file.

**Pagination Delta Parameter Name:** Set the name of the URL parameter that stores the Pagination Delta value (`delta` by default). This parameter is visible in the browser if the User changes the number. If the User selects 10 results per page and searches for _test_, the Search Page is reloaded with this URL:

```log
http://localhost:8080/web/guest/search?q=test&delta=10
```

### [Displaying Results from Alternate Indexes](https://learn.liferay.com/web/guest/w/dxp/using-search/search-pages-and-widgets/search-results/configuring-the-search-results-widget#displaying-results-from-alternate-indexes) <a href="#displaying-results-from-alternate-indexes" id="displaying-results-from-alternate-indexes"></a>

**Federated Search Key:** If this widget is participating in a search on a non-default index, enter the key of the alternate search index. If not set, the widget participates in the default search, against the default index (`liferay-[companyId]`). This value is usually the name of an application-defined index. See examples in [Understanding Low Level Search Options](https://learn.liferay.com/web/guest/w/dxp/using-search/search-pages-and-widgets/search-results/understanding-low-level-search-options).

**Fields to Display:** If searching an [alternate index](https://learn.liferay.com/web/guest/w/dxp/using-search/search-pages-and-widgets/search-results/understanding-low-level-search-options) using the Federated Search Key configuration, specify the fields to search from that index. Fields must be both indexed and [stored](https://www.elastic.co/guide/en/elasticsearch/reference/8.8/mapping-store.html) to appear in the search Results widget.

### [Inspecting Search Engine Documents](https://learn.liferay.com/web/guest/w/dxp/using-search/search-pages-and-widgets/search-results/configuring-the-search-results-widget#inspecting-search-engine-documents) <a href="#inspecting-search-engine-documents" id="inspecting-search-engine-documents"></a>

WARNING

Never use this option in production environments.

**Display Results in Document Form:** Display results as search [documents](https://learn.liferay.com/web/guest/w/dxp/using-search/developer-guide) to [Instance Administrators](https://learn.liferay.com/web/guest/w/dxp/users-and-permissions/roles-and-permissions/understanding-roles-and-permissions) (other registered Users and unauthenticated Guest Users cannot see the document view). Use this feature while developing your site to view search responses in their indexed, document-based format. Part of writing search indexers is to convert search engine documents (the objects that get indexed) to the actual Java object and back again. To use this setting, enable it and then click the _Details…_ link below a Search Results summary: the result’s document view is expanded for your inspection.

![Viewing a results document lets Instance Administrators inspect exactly what's being indexed for a particular asset. This is just a small portion of one document.](https://resources.learn.liferay.com/images/dxp/latest/en/using-search/search-pages-and-widgets/search-results/configuring-the-search-results-widget/images/05.png)

For further reading, check out how to [return suggestions for better search terms](https://learn.liferay.com/web/guest/w/dxp/using-search/search-pages-and-widgets/search-results/enabling-search-suggestions) (for example, “Did you mean…”) when not enough results are returned initially.
