# Understanding Low Level Search Options

Low level search doesn’t go through the Search and Indexing Framework, which is infrastructure used for searching documents in a Liferay search index.

A common use case for a low level search is to query an index other than the Liferay DXP index. By default, [Search Pages](https://learn.liferay.com/web/guest/w/dxp/using-search/search-pages-and-widgets/working-with-search-pages/search-pages) search the Liferay DXP index, but you can also search another index, as long as it’s in the same Elasticsearch cluster.

Add the Low Level Search Options widget to a search page and configure it to direct the search to the alternate index. To search multiple indexes from the same page, you can add multiple Low Level Search Options widgets and configure each one with its own Index Name and Federated Search Key.

Searching alternate indexes is a low level operation that bypasses the Liferay DXP permission checking mechanisms, presenting whatever results the search engine returns. For this reason, only administrators can add and configure the Low Level Search Options widget.

To use the Low Level Search Options widget, add it to a Search Page:

1.  Click the Add icon (![Add app icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add-widget.png)) on the page to open the Add Widgets menu.

    ![The low level search options widget is under the search widget.](https://resources.learn.liferay.com/images/dxp/latest/en/using-search/search-pages-and-widgets/search-results/understanding-low-level-search-options/images/01.png)
2. Drag the Low Level Search Options widget (from the Search section) and drop it on the page.
3.  Click the Options icon (![Options icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-app-options.png)) and click _Configuration_.

    ![Click on the configuration link to open the widget configuration.](https://resources.learn.liferay.com/images/dxp/latest/en/using-search/search-pages-and-widgets/search-results/understanding-low-level-search-options/images/02.png)

The widget has different options to configure:

**\[Liferay 7.3+] Connection ID:** Select the Connection ID of the connection to be used to perform the search.

**Indexes:** Enter the comma-separated names of the alternative indexes to search. Usually, you shouldn’t enter a Liferay company index name.

**Fields to Return:** Enter the names of the stored fields to be returned from the search engine in a comma-separated list. Leave it blank to return all stored fields.

**Contributors to Include:** Enter the IDs of registered search contributors to be included in this search in a comma-separated list of each `SearchRequestContributor`’s Fully Qualified Class Name (e.g., `com.liferay.docs.request.contributor.MySearchRequestContributor`). If not set, all registered search contributors are applied.

**Contributors to Exclude:** Enter the IDs of registered search contributors to be excluded from this search, in a comma-separated list. If not set, all registered search contributors are applied.

NOTE

These _Contributors_ are components implementing the `com.liferay.portal.search.spi.searcher.SearchRequestContributor` interface (provided by the `com.liferay.portal.search.spi` artifact), which is an extension point (SPI) that intercepts search requests and adds query parts.

**Federated Search Key:** Enter the key of an alternate search this widget is participating in. If not set, this widget participates in the default search. This value is usually the name of an application-defined index.

**\[Liferay 7.4+] Attributes:** Add key/value attributes to the search context. This section is most useful for [applying a Search Blueprint](https://learn.liferay.com/web/guest/w/dxp/using-search/liferay-enterprise-search/search-experiences/search-blueprints/using-a-search-blueprint-on-a-search-page) to a Search Page. It can also be used for development and testing purposes: add attributes to the page’s search context that can be read in your custom code (e.g., `searchContext.getAttribute("key")`).

### [Displaying Low Level Search Results](https://learn.liferay.com/web/guest/w/dxp/using-search/search-pages-and-widgets/search-results/understanding-low-level-search-options#displaying-low-level-search-results) <a href="#displaying-low-level-search-results" id="displaying-low-level-search-results"></a>

Assets indexed into a Liferay company index (e.g, `liferay-20097`) can implement a `ModelSummaryContributor` to provide their own display logic that’s used by the Search Results widget. Low level searches use different display logic since there’s no summary to draw from:

* If only one field is configured, it appears as the title field (large, bold text is presented).
* If two fields are present, the first is used as the title (large, bold text) and the second is the description field.
* If more than two fields are present, the first is used as the title field (with large, bold text), and the rest are mashed up as a description field.

![The Low Level Search Framework contains logic to produce attractive result summaries on the fly.](https://resources.learn.liferay.com/images/dxp/latest/en/using-search/search-pages-and-widgets/search-results/understanding-low-level-search-options/images/04.png)

### [Low Level Search and Permission Checking](https://learn.liferay.com/web/guest/w/dxp/using-search/search-pages-and-widgets/search-results/understanding-low-level-search-options#low-level-search-and-permission-checking) <a href="#low-level-search-and-permission-checking" id="low-level-search-and-permission-checking"></a>

Permissions checking is not available via low level search. If you search even a company index using low level search, important [permission checking](https://learn.liferay.com/web/guest/w/dxp/using-search/search-pages-and-widgets/search-results/search-results-behavior#permissions-and-search-results) is circumvented. You almost always want the benefits of permission checking, but one possible use case for intentionally bypassing the permission checks is an admin-only search page where anyone accessing the search page must have access to all results in the company index.

### [Low Level Search and Relevance](https://learn.liferay.com/web/guest/w/dxp/using-search/search-pages-and-widgets/search-results/understanding-low-level-search-options#low-level-search-and-relevance) <a href="#low-level-search-and-relevance" id="low-level-search-and-relevance"></a>

Relevance scoring only makes sense within an index. Results coming from separate indexes cannot be accurately scored relative to each other. Therefore, it’s best to display the results from each index in a separate Search Results widget. The example below demonstrates this.

### [Example 1: Searching an Alternate Index](https://learn.liferay.com/web/guest/w/dxp/using-search/search-pages-and-widgets/search-results/understanding-low-level-search-options#example-1-searching-an-alternate-index) <a href="#example-1-searching-an-alternate-index" id="example-1-searching-an-alternate-index"></a>

1. Whether testing on the default search page or creating a new [Search Page](https://learn.liferay.com/web/guest/w/dxp/using-search/search-pages-and-widgets/working-with-search-pages/search-pages), include the following widgets (removing extra widgets simplifies the exercise, but is not required for it to work):
   * Low Level Search Options
   * Custom Filter
   * Search Bar
   * Search Results
2.  Configure all the widgets to participate in an alternate search by clicking the Options icon (![Options icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-app-options.png)) and selecting _Configuration_. For each, enter _liferay-0_ in the Federated Search Key setting.

    All the search widgets expected to react appropriately to the alternate search must be configured with the Federated Search Key.
3.  Make an additional configuration in the Low Level Search Options widget, adding the index name of the alternate index:

    Enter at least one index name in the _Indexes_ setting. To follow this example, use _liferay-0_.
4.  Configure the Custom Filter to use the search bar’s default query parameter (_q_) and add a query to the search:

    Enter _q_ in the Custom Parameter Name field.

    Enter _title_ under Filter Field to add the title field to the query.

    Choose _Match_ under Filter Query Type.

    Since you’re overriding the default query to search an alternate index, there’s nothing in the query by default. Any query clauses you want sent to the search engine must be added manually using [Custom Filter](https://learn.liferay.com/web/guest/w/dxp/using-search/search-pages-and-widgets/search-results/using-the-custom-filter-widget) widget(s).

    IMPORTANT

    Set the Custom Parameter Name to `q` to match the default value in the Search Bar’s Keywords Parameter Name setting. The matching parameter ensures that the user’s keywords, entered into the search bar, are applied to the Custom Filter widget. If a custom value is set in the Keywords Parameter Name of the Search Bar, use that instead of `q`.

If you’re following the example here to search _liferay-0_, search for _dynamic_ in the search bar. You’ll see results like this:

![Example of results from low level search.](https://resources.learn.liferay.com/images/dxp/latest/en/using-search/search-pages-and-widgets/search-results/understanding-low-level-search-options/images/03.png)

### [Example 2: Searching the Liferay Company Index and an Additional Index](https://learn.liferay.com/web/guest/w/dxp/using-search/search-pages-and-widgets/search-results/understanding-low-level-search-options#example-2-searching-the-liferay-company-index-and-an-additional-index) <a href="#example-2-searching-the-liferay-company-index-and-an-additional-index" id="example-2-searching-the-liferay-company-index-and-an-additional-index"></a>

Set up a [Search Page](https://learn.liferay.com/web/guest/w/dxp/using-search/search-pages-and-widgets/working-with-search-pages/search-pages) that displays one set of results from Liferay’s company index (e.g, `liferay-20098`) and another from some alternate index of interest in [Liferay’s Elasticsearch cluster](https://learn.liferay.com/web/guest/w/dxp/using-search/installing-and-upgrading-a-search-engine/elasticsearch) (this example uses `liferay-0` for demonstration purposes):

1.  Add these widgets to a [new page called _Low Level Search_](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/using-widget-pages/adding-widgets-to-a-page):

    * One Low Level Search Options
    * Two [Search Bars](https://learn.liferay.com/web/guest/w/dxp/using-search/getting-started/searching-for-content#using-the-search-bar)
    * Two [Search Results](https://learn.liferay.com/web/guest/w/dxp/using-search/search-pages-and-widgets/search-results/configuring-the-search-results-widget)
    * One [Custom Filter](https://learn.liferay.com/web/guest/w/dxp/using-search/search-pages-and-widgets/search-results/using-the-custom-filter-widget)

    TIP

    The 1-2-1 Columns (50/50) [page layout](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-settings/configuring-individual-pages#page-layout) is convenient for this example.
2.  Configure the widgets:

    | Search Widget            | Widget Configuration                                                                                                                                                                                                                                                                     | Explanation                                                                                                                                                                                                                                                                                                          |
    | ------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Low Level Search Options | <p><strong>Indexes:</strong> <code>liferay-0</code><br><strong>Federated Search Key:</strong> <code>liferay-0</code></p>                                                                                                                                                                 | This sets up the search for an alternate index. In this case it’s another Liferay controlled index, for demonstration purposes only. The Federated Search Key can be any memorable string. Since this example searches only for a single additional index, it’s named identically to the index itself.               |
    | Search Results (first)   | -                                                                                                                                                                                                                                                                                        | Out of the box, the Search Results widget is pre-configured for the Liferay company index, so this one can be left with all the defaults.                                                                                                                                                                            |
    | Search Results (second)  | **Federated Search Key:** `liferay-0`                                                                                                                                                                                                                                                    | There are additional configurations, like controlling which fields to display. Leaving this option blank asks [Liferay to figure it out](https://learn.liferay.com/web/guest/w/dxp/using-search/search-pages-and-widgets/search-results/understanding-low-level-search-options#displaying-low-level-search-results). |
    | Search Bar (first)       | -                                                                                                                                                                                                                                                                                        | Leave the defaults in the first Search Bar, since it’s configured to search the company index out of the box.                                                                                                                                                                                                        |
    | Search Bar (second)      | <p><strong>Invisible:</strong> <code>true</code><br><strong>Federated Search Key:</strong> <code>liferay-0</code></p>                                                                                                                                                                    | Only one Search Bar is used for user input, so this one should be invisible. Importantly, you left the default value for Keywords Parameter Name in both Search Bar widgets. This Search Bar can ingest the search terms entered by users because it shares the parameter with the visible Search Bar on the page.   |
    | Custom Filter            | <p><strong>Filter Field:</strong> <code>title_en_US</code><br><strong>Occur:</strong> <code>should</code><br><strong>Custom Parameter Name:</strong><code>q</code><br><strong>Invisible:</strong> <code>true</code><br><strong>Federated Search Key:</strong> <code>liferay-0</code></p> | The Custom Filter widget should match the title field of the `liferay-0` index. If you want more filters, add more Custom Filter widgets to the page.                                                                                                                                                                |

    IMPORTANT

    Set the Custom Parameter Name to `q` to match the default value in the Search Bar’s Keywords Parameter Name setting. The matching parameter ensures that the user’s keywords, entered into the search bar, are applied to the Custom Filter widget. If a custom value is set in the Keywords Parameter Name of the Search Bar, use that instead of `q`.

To test out the Low Level Search page functionality,

1. Add at least one asset in the Site (e.g., Blogs Entries, Web Content Articles, etc.) and include the word _content_ in any assets you create.
2. In the Search Bar on the Low Level Search page, enter _content_ and see that you get results from both indexes.

![Results from disparate indexes are best displayed in separate Search Results widgets.](https://resources.learn.liferay.com/images/dxp/latest/en/using-search/search-pages-and-widgets/search-results/understanding-low-level-search-options/images/05.png)

Now you’re able to configure the out of the box search widgets to participate in searches against any Elasticsearch index in the cluster.
