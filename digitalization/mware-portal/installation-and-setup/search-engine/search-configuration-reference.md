# Search Configuration Reference

Search in Liferay DXP can be configured in lots of ways. Once you understand the concept of [configuration scope](https://learn.liferay.com/web/guest/w/dxp/system-administration/configuring-liferay/understanding-configuration-scope), you’ll understand where to find a certain configuration screen.

* Many search configurations affect the system scope (e.g., re-indexing actions and configuring the search engine connector).
* Search tuning configurations affect the virtual instance.
* Creating new Search Pages affects search at the site scope.
* Configuring the search widgets is widget-scoped configuration, but some can also be thought of as page-scoped configuration.

So the term `Configuring Search` is very broad, as it addresses all the available scopes. This is a high level overview of what search behavior is configurable out of the box, and importantly, where to find search configuration options.

### [Widget Scoped Search Configuration](https://learn.liferay.com/web/guest/w/dxp/using-search/search-configuration-reference#widget-scoped-search-configuration) <a href="#widget-scoped-search-configuration" id="widget-scoped-search-configuration"></a>

Several search widgets are available to place on Pages. Each one has its own configuration options.

![Each widget's configuration will be unique.](https://resources.learn.liferay.com/images/dxp/latest/en/using-search/search-configuration-reference/images/03.png)

#### [Search Widgets](https://learn.liferay.com/web/guest/w/dxp/using-search/search-configuration-reference#search-widgets) <a href="#search-widgets" id="search-widgets"></a>

For dedicated coverage of the search widgets, see [Search Pages and Widgets](https://learn.liferay.com/web/guest/w/dxp/using-search/getting-started/search-overview#search-pages-and-widgets).

**Search Results**: Configure how search results are displayed. Read [Displaying Search Results](https://learn.liferay.com/web/guest/w/dxp/using-search/search-pages-and-widgets/search-results/configuring-the-search-results-widget#displaying-search-results) for more information.

**Search Bar**: Configure the behavior of how search keywords are processed. See [Configuring the Search Bar](https://learn.liferay.com/web/guest/w/dxp/using-search/getting-started/searching-for-content#configuring-the-search-bar) for more information.

**Search Facets**: Configure each facet’s behavior and URL parameters. See [Facets](https://learn.liferay.com/web/guest/w/dxp/using-search/search-pages-and-widgets/search-facets) for more information.

**Search Options**: This is a special case, where configuring this widget defines page scoped behavior. Add the Search Option widget to a page and define two booleans for the Search Page:

* Allow Empty Searches: By default, failure to enter a keyword returns no results. Enabling this ensure that _all_ results are returned when no keyword is entered in the Search Bar.
* Basic Facet Selection: By default, facet counts are recalculated after each facet selection. Enable this to turn off facet recounting.

**Search Suggestions**: Suggest better queries and spell check queries. See the [Suggestions Configuration Reference](https://learn.liferay.com/web/guest/w/dxp/using-search/search-pages-and-widgets/search-results/enabling-search-suggestions#suggestions-configuration-reference) for more information.

**Search Insights**: Add this to the Search Page to inspect the full query string that’s constructed by the back-end search code when the User enters a keyword. Only useful for testing and development. See [Search Insights](https://learn.liferay.com/web/guest/w/dxp/using-search/search-pages-and-widgets/search-insights) for more information.

**Custom Filter**: Add a widget to the page for each of the filters you’d like applied to the search results. Let search page users see and manipulate the filters or make them invisible and/or immutable. See [Using the Custom Filter Widget](https://learn.liferay.com/web/guest/w/dxp/using-search/search-pages-and-widgets/search-results/using-the-custom-filter-widget) for more information.

**Sort**: Let Users reorder the search results based on the value of certain `keyword` fields in the index. For example, show results in alphabetic order of the Title field. The default order is determined by the search engine’s _Relevance_ calculation. See [Sorting Search Results](https://learn.liferay.com/web/guest/w/dxp/using-search/search-pages-and-widgets/search-results/sorting-search-results) for more information.

**Low Level Search Options:** Configure the search widgets to participate in a search that’s aimed at an index other than the _Company Index_. The company index is where the Liferay DXP assets index their data, so many installations will not need this widget. See [Understanding Low Level Search Options](https://learn.liferay.com/web/guest/w/dxp/using-search/search-pages-and-widgets/search-results/understanding-low-level-search-options) for more information.

**Similar Results:** Display similar search results to an asset being displayed on a page. See [Similar Results](https://learn.liferay.com/web/guest/w/dxp/using-search/search-pages-and-widgets/similar-results) for more information.

\[Liferay Enterprise Search] **Elasticsearch Monitoring:** Liferay Enterprise Search subscribers can access [Elastic’s Kibana](https://www.elastic.co/kibana) monitoring tool inside a widget placed on a Liferay DXP Page. See [Monitoring Elasticsearch](https://learn.liferay.com/web/guest/w/dxp/using-search/liferay-enterprise-search/monitoring-elasticsearch) for more information.

### [Site Scoped Search Configuration](https://learn.liferay.com/web/guest/w/dxp/using-search/search-configuration-reference#site-scoped-search-configuration) <a href="#site-scoped-search-configuration" id="site-scoped-search-configuration"></a>

By the strict definition of [Site Scoped Configuration](https://learn.liferay.com/web/guest/w/dxp/system-administration/configuring-liferay/understanding-configuration-scope), search doesn’t have any. However, [Search Pages](https://learn.liferay.com/web/guest/w/dxp/using-search/search-pages-and-widgets/working-with-search-pages/search-pages) influence Site-specific search behavior. Commonly, Search Pages contain search widgets configured to search for all content within a particular Site.

![Configure the scope of a search.](https://resources.learn.liferay.com/images/dxp/latest/en/using-search/search-configuration-reference/images/04.png)

There are some important configuration nuances to be aware of when using the Search widgets:

If the header Search Bar (the search bar embedded in the default theme) uses the Search Bar widget, its configuration always requires a _destination page_ to be set, where Users are redirected to complete their search activity, interacting with the other Search widgets (Results, Facets, Suggestions etc.). [Search destination pages](https://learn.liferay.com/web/guest/w/dxp/using-search/search-pages-and-widgets/working-with-search-pages/creating-a-search-page) are ordinary pages holding the Search widgets. You can have as many pages with Search widgets across the Site as you want.

The Search Bar widget is instanceable, so one page can contain multiple Search Bar widgets configured differently. All Search Bar instances must point to a Search Page within the Site to be effective.

IMPORTANT

If the destination Search Page has a Search Bar widget instance in addition to a theme-embedded header Search Bar (like Liferay’s default theme), the configurations of the header Search Bar take precedence over the page’s widget instance.

Conversely, searching from a Search Bar widget instance on other pages honors their configurations, even if they differ from the header Search Bar configuration.

See [Configuring the Search Bar](https://learn.liferay.com/web/guest/w/dxp/using-search/getting-started/searching-for-content#configuring-the-search-bar) for more information.

### [Instance Scoped Search Configuration](https://learn.liferay.com/web/guest/w/dxp/using-search/search-configuration-reference#instance-scoped-search-configuration) <a href="#instance-scoped-search-configuration" id="instance-scoped-search-configuration"></a>

Search does not have any Instance scoped entries in the [Instance Settings](https://learn.liferay.com/web/guest/w/dxp/system-administration/configuring-liferay/understanding-configuration-scope#system-settings-and-instance-settings) panel. However, there are entries for configuring the instance-wide default [widget template](https://learn.liferay.com/web/guest/w/dxp/site-building/displaying-content/additional-content-display-options/styling-widgets-with-widget-templates) for many search widgets. Just enter the Site ID where the template is defined (often this will be the Global site’s ID, found in its Site Settings panel) and the Widget Template ID, found in the Site Menu → Design → Widget Templates.

The only instance scoped configurations for search are the entries in the Global Menu → Applications → Search Tuning:

**Result Rankings:** Customize search results manually by hiding, pinning, and adding results for specific aliases. See [Result Rankings](https://learn.liferay.com/web/guest/w/dxp/using-search/search-administration-and-tuning/result-rankings) for more information.

**Synonyms:** Create synonym sets so that synonymous search terms are matched and scored like an exact match to the search term. See [Synonym Sets](https://learn.liferay.com/web/guest/w/dxp/using-search/search-administration-and-tuning/synonym-sets) for more information.

### [System Scoped Search Configuration](https://learn.liferay.com/web/guest/w/dxp/using-search/search-configuration-reference#system-scoped-search-configuration) <a href="#system-scoped-search-configuration" id="system-scoped-search-configuration"></a>

System scoped search configurations are primarily found in [System Settings](https://learn.liferay.com/web/guest/w/dxp/system-administration/configuring-liferay/system-settings).

1. In the Global Menu, go to _Control Panel_ → _Configuration_ → _System Settings_.
2.  Click the _Search_ category under the Platform section.

    Alternatively, search for _search_.

![There are numerous system scoped entries for search in System Settings.](https://resources.learn.liferay.com/images/dxp/latest/en/using-search/search-configuration-reference/images/01.png)

System Settings can also be configured via [OSGi configuration file](https://learn.liferay.com/web/guest/w/dxp/system-administration/configuring-liferay/configuration-files-and-factories/using-configuration-files). The file names re included here.

#### [Default Keyword Query](https://learn.liferay.com/web/guest/w/dxp/using-search/search-configuration-reference#default-keyword-query) <a href="#default-keyword-query" id="default-keyword-query"></a>

**Configuration File:** `com.liferay.portal.search.configuration.DefaultKeywordQueryConfiguration.config`

The Default Keyword Query entry contains one setting:

**`disabledEntryClassNames`**: The `DefaultKeywordQueryContributor` code automatically adds `description`, `userName`, and `title` fields to the keyword search query. Specify the entry class names `DefaultKeywordQueryContributor` should ignore.

#### [Default Search Result Permission Filter](https://learn.liferay.com/web/guest/w/dxp/using-search/search-configuration-reference#default-search-result-permission-filter) <a href="#default-search-result-permission-filter" id="default-search-result-permission-filter"></a>

**Configuration File:** `com.liferay.portal.search.configuration.DefaultSearchResultPermissionFilterConfiguration.config`

The Default Search Result Permission Filter entry allows configuration of _post-filtering permission checking_ (database permission checking that occurs after the results are returned from the search index). Read [Final Permissions Checking](https://learn.liferay.com/web/guest/w/dxp/using-search/search-pages-and-widgets/search-results/search-results-behavior#final-permissions-checking) for more information on these settings:

* **`permissionFilteredSearchResultAccurateCountThreshold`**
* **`searchQueryResultWindowLimit`**

#### [Index Status Manager](https://learn.liferay.com/web/guest/w/dxp/using-search/search-configuration-reference#index-status-manager) <a href="#index-status-manager" id="index-status-manager"></a>

**Configuration File:** `com.liferay.portal.search.configuration.IndexStatusManagerConfiguration.config`

The Index Status Manager entry has one setting:

**`indexReadOnly`**: Enable this to suspend all indexing operations and writes to the search engine. Searches return only the documents already indexed. This is useful for speeding up large data imports, but it should be disabled and a full re-index executed once the import is finished.

#### [Indexer Writer Helper](https://learn.liferay.com/web/guest/w/dxp/using-search/search-configuration-reference#indexer-writer-helper) <a href="#indexer-writer-helper" id="indexer-writer-helper"></a>

**Configuration File:** `com.liferay.portal.search.configuration.IndexWriterHelperConfiguration.config`

The Index Writer Helper entry contains one entry:

**`indexCommitImmediately`**: When _true_ (the default), each write request forces the search engine to refresh the index reader, potentially flushing transactions to disk. This may negatively impact search engine performance. The default behavior is to commit immediately for index writing on individual assets (e.g. add blog, update blog) but delay commits for bulk index writing operations (e.g. index all users, index all form entries) until all entries have been sent to the search engine. Setting this to false changes the behavior for individual index operations, and may cause applications like Asset Publisher to exhibit a delayed response when showing newly added content. See the [Elasticsearch documentation](https://www.elastic.co/guide/en/elasticsearch/reference/8.8/near-real-time.html) for more information.

#### [Indexer Registry](https://learn.liferay.com/web/guest/w/dxp/using-search/search-configuration-reference#indexer-registry) <a href="#indexer-registry" id="indexer-registry"></a>

NOTE

For Liferay 7.4 Update/GA31+, the Indexer Registry configuration isn’t available in the System Settings UI. You must configure it with a `.config` file.

**Configuration File:** `com.liferay.portal.search.configuration.IndexerRegistryConfiguration.config`

Configure the buffering of index requests:

**`buffered`**: Enable (the default) or disable the buffering of indexing requests. Disabling buffering is not recommended: more requests to Elasticsearch must be made, and index changes are not rolled back when the database is rolled back.

**`bufferedExecutionMode`**: Allow administrators to select a different `IndexerRequestBufferExecutor`, used to execute a `IndexerRequest`. One implementation of the executor is provided out of the box (_DEFAULT_). When a new `IndexerRequestBufferExecutor` implementation is deployed, one of the required properties is a `buffered.execution.mode`. This value can then be used in place of _DEFAULT_.

**`maximumBufferSize`**: If buffering is enabled, set the Maximum Buffer Size so that any additional indexing requests are executed immediately.

**`minimumBufferAvailabilityPercentage`**: When the capacity of the buffer has only the specified percent of space left, the existing requests in the buffer are executed in one batch and removed from the buffer.

#### [Index Query Preprocessor](https://learn.liferay.com/web/guest/w/dxp/using-search/search-configuration-reference#index-query-preprocessor) <a href="#index-query-preprocessor" id="index-query-preprocessor"></a>

**Configuration File:** `com.liferay.portal.search.configuration.QueryPreProcessConfiguration.config`

This entry has one repeatable property (use array syntax if you’re defining via [OSGi configuration file](https://learn.liferay.com/web/guest/w/dxp/system-administration/configuring-liferay/configuration-files-and-factories/using-configuration-files#creating-configuration-files)):

**`fieldNamePatterns`**: Fields with names matching the patterns set here are treated as non-analyzed keyword fields. Instead of scored full text queries, matching is performed by non-scored wildcard queries. This is a resource intensive operation that degrades search engine performance as indexes grow larger. For substring matching, relying on the [ngram tokenizer](https://www.elastic.co/guide/en/elasticsearch/reference/current/analysis-ngram-tokenizer.html) usually performs better.

#### [Reindex](https://learn.liferay.com/web/guest/w/dxp/using-search/search-configuration-reference#reindex) <a href="#reindex" id="reindex"></a>

**Configuration File:** `com.liferay.portal.search.configuration.ReindexConfiguration.config`

**`defaultReindexExecutionMode`**: Set the default mode for re-indexing Liferay’s content. Choose from Full (default), Concurrent, or Sync. See [Re-Indexing Modes](https://learn.liferay.com/web/guest/w/dxp/using-search/search-administration-and-tuning/re-indexing-modes) for more information.

**`indexingBatchSizes`**: Set the number of documents (the default value is 1000) indexed per batch for model types that support batch indexing. For models with large documents, decreasing this value may improve stability when executing a full re-index.

IMPORTANT

To configure indexing batch size for web content articles, the model to specify depends on a Web Content setting. If `indexAllArticleVersionsEnabled` is `true` in System Settings → Content & Data → Web Content → Web Content (virtual instance scope), you must set `com.liferay.journal.model.JournalArticle==[size]`. If disabled, use `com.liferay.journal.model.JournalArticleResource=[size]`.

#### [Engine Helper](https://learn.liferay.com/web/guest/w/dxp/using-search/search-configuration-reference#engine-helper) <a href="#engine-helper" id="engine-helper"></a>

**Configuration File:** `com.liferay.portal.search.configuration.SearchEngineHelperConfiguration.config`

This entry has one repeatable property (use array syntax if you’re defining via [OSGi configuration file](https://learn.liferay.com/web/guest/w/dxp/system-administration/configuring-liferay/configuration-files-and-factories/using-configuration-files)):

**`excludedEntryClassNames`**: Exclude an asset type from being searched in the catchall query constructed for the Search application. For example, fields of the Organization asset must be indexed to be searchable from the Users and Organizations application, but should not be searched in the Search application. Thus, Organizations are added to `excludedEntryClassNames`.

#### [Permission Checker](https://learn.liferay.com/web/guest/w/dxp/using-search/search-configuration-reference#permission-checker) <a href="#permission-checker" id="permission-checker"></a>

**Configuration File:** `com.liferay.portal.search.configuration.SearchPermissionCheckerConfiguration.config`

This entry has one configuration option in Liferay DXP 7.3:

**`permissionTermsLimit`**: Limits the number of permission search clauses added to the search query before this level of permission checking is aborted. Permission checking then relies solely on the final permission filtering described in the [Default Search Result Permission Filter](https://learn.liferay.com/web/guest/w/dxp/using-search/search-configuration-reference#default-search-result-permission-filter) section.

#### [Title Field Query Builder](https://learn.liferay.com/web/guest/w/dxp/using-search/search-configuration-reference#title-field-query-builder) <a href="#title-field-query-builder" id="title-field-query-builder"></a>

**Configuration File:** `com.liferay.portal.search.configuration.TitleFieldQueryBuilderConfiguration.config`

Configure how search responds to matches on the Title field of a document.

**`exactMatchBoost`**: Give an additional boost when searched keywords exactly match the `title` field of a document.

**`maxExpansions`**: Limit the number of documents to return when matching searched keywords to the `title` field as a phrase prefix. See Elasticsearch’s [Match Phrase Query documentation](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-match-query-phrase.html) for more information.

#### [Cross-Cluster Replication](https://learn.liferay.com/web/guest/w/dxp/using-search/search-configuration-reference#cross-cluster-replication) <a href="#cross-cluster-replication" id="cross-cluster-replication"></a>

You’ll only see this entry if you have deployed the Cross-Cluster Replication module, which is available with a Liferay Enterprise Search subscription. These configuration properties are covered in the dedicated [Cross-Cluster Replication](https://learn.liferay.com/web/guest/w/dxp/using-search/liferay-enterprise-search/cross-cluster-replication/cross-cluster-replication) documentation.

#### [Elasticsearch Monitoring](https://learn.liferay.com/web/guest/w/dxp/using-search/search-configuration-reference#elasticsearch-monitoring) <a href="#elasticsearch-monitoring" id="elasticsearch-monitoring"></a>

**Configuration File:** `com.liferay.portal.search.elasticsearch.monitoring.web.internal.configuration.MonitoringConfiguration.config`

You can use the Elasticsearch Monitoring configuration if you have deployed the Liferay Enterprise Search Monitoring module, available with a Liferay Enterprise Search subscription. These configuration properties are covered in the dedicated [Monitoring Elasticsearch](https://learn.liferay.com/web/guest/w/dxp/using-search/liferay-enterprise-search/monitoring-elasticsearch) documentation.

#### [Elasticsearch 7](https://learn.liferay.com/web/guest/w/dxp/using-search/search-configuration-reference#elasticsearch-7) <a href="#elasticsearch-7" id="elasticsearch-7"></a>

IMPORTANT

The _Elasticsearch 7_ system settings entry and corresponding configuration file are also used for configuring the connection to Elasticsearch 8.

**Configuration File:** `com.liferay.portal.search.elasticsearch7.configuration.ElasticsearchConfiguration.config`

Configure the connection between Liferay and Elasticsearch. See [Configuring the Connector](https://learn.liferay.com/web/guest/w/dxp/using-search/installing-and-upgrading-a-search-engine/elasticsearch/connecting-to-elasticsearch#configuring-the-connector) for detailed coverage of these properties.

#### [Elasticsearch Connections](https://learn.liferay.com/web/guest/w/dxp/using-search/search-configuration-reference#elasticsearch-connections) <a href="#elasticsearch-connections" id="elasticsearch-connections"></a>

**Configuration File:** `com.liferay.portal.search.elasticsearch7.configuration.ElasticsearchConnectionConfiguration-[connectionId].config`

Liferay 7.3 can connect to multiple Elasticsearch clusters. Connections are defined in the Elasticsearch Connections entry. This feature can be used in conjunction with the Low Level Search Options and Search Results widgets to show results from third-party systems using Elasticsearch clusters. It’s also demonstrated in the [Cross-Cluster Replication](https://learn.liferay.com/web/guest/w/dxp/using-search/liferay-enterprise-search/cross-cluster-replication/cross-cluster-replication) documentation.

#### [Learning to Rank](https://learn.liferay.com/web/guest/w/dxp/using-search/search-configuration-reference#learning-to-rank) <a href="#learning-to-rank" id="learning-to-rank"></a>

**Configuration File:** `com.liferay.portal.search.learning.to.rank.configuration.LearningToRankConfiguration.config`

You can use the Learning to Rank configuration if you have deployed the Liferay Enterprise Search Learning to Rank module, available with a Liferay Enterprise Search subscription. These configuration properties are covered in the dedicated [Learning to Rank](https://learn.liferay.com/web/guest/w/dxp/using-search/liferay-enterprise-search/learning-to-rank) documentation.

#### [Search Web](https://learn.liferay.com/web/guest/w/dxp/using-search/search-configuration-reference#search-web) <a href="#search-web" id="search-web"></a>

**Configuration File:** `com.liferay.portal.search.web.internal.configuration.SearchWebConfiguration.config`

This entry contains one property:

**`classicSearchPortletInFrontPage`**: Revert the default search experience from using the new Search Widgets to the deprecated classic Search Portlet that was standard in past releases.

#### [Synonyms](https://learn.liferay.com/web/guest/w/dxp/using-search/search-configuration-reference#synonyms) <a href="#synonyms" id="synonyms"></a>

**Configuration File:** `com.liferay.portal.search.tuning.synonyms.web.internal.configuration.SynonymsConfiguration.config`

This entry has one repeatable property (use array syntax if you’re defining via [OSGi configuration file](https://learn.liferay.com/web/guest/w/dxp/system-administration/configuring-liferay/configuration-files-and-factories/using-configuration-files#creating-configuration-files)):

**`synyonymFilterNames`**: Set the synonym filter names to apply. These filters must be defined in the Elasticsearch Index Settings. See [Creating New Synonym Language Filters](https://learn.liferay.com/web/guest/w/dxp/using-search/search-administration-and-tuning/synonym-sets#creating-new-synonym-language-filters).

#### [Search Administration](https://learn.liferay.com/web/guest/w/dxp/using-search/search-configuration-reference#search-administration) <a href="#search-administration" id="search-administration"></a>

In _Control Panel_ → _Configuration_ → _Search_ there are three administrative UIs: Connections, Index Actions, and Field Mappings.

See the dedicated [Search Administration and Tuning](https://learn.liferay.com/web/guest/w/dxp/using-search/search-administration-and-tuning) documentation for more details.

[**Connections**](https://learn.liferay.com/web/guest/w/dxp/using-search/search-configuration-reference#connections)

Information about the search engine connection(s) is displayed here. For example,

* Search Engine Vendor: Elasticsearch
* Client Version: 7.16.3
* Active Connections: 1
* Health: Green
* Connection ID: \_REMOTE\_
* Connection Type: Read/Write
* Cluster Name: LiferayElasticsearchCluster
* Nodes: 1
* Node Name: lr-es
* Node Version: 7.17.14

[**Index Actions**](https://learn.liferay.com/web/guest/w/dxp/using-search/search-configuration-reference#index-actions)

In Index Actions, re-index at one of these levels:

* All indexable assets
* An individual indexable asset
* All spell check indexes

[**Field Mappings**](https://learn.liferay.com/web/guest/w/dxp/using-search/search-configuration-reference#field-mappings)

#### [Portal Properties](https://learn.liferay.com/web/guest/w/dxp/using-search/search-configuration-reference#portal-properties) <a href="#portal-properties" id="portal-properties"></a>

Portal properties are system scoped as well. The [Lucene Search](https://learn.liferay.com/reference/latest/en/dxp/propertiesdoc/portal.properties.html#Lucene%20Search) portal properties configure low level search behavior. Review the properties and their descriptions and determine if they apply to your search requirements.

\
