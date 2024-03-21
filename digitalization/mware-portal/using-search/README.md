# Using Search

Search is a fundamental component of the portal. Elasticsearch is bundled with the portal for testing and development purposes. Production environments require Elasticsearch running on a remote server. Getting Started with Elasticsearch explains remote server setup. You can explore the search functionality immediately, however, using the bundled Elasticsearch engine.

Here are the search features:

* Full-text search
* Indexing of all content types (blogs, documents, web content, etc.)
* Highly configurable search pages
* A search bar embedded in the header of every page
* Automatic index synchronization for added, updated, and deleted content
* Search result filtering by roles and permissions
* Search suggestions
* Search result configuration (e.g., filter and sort)
* Faceted search
* Enable or disable advanced search syntax (e.g., AND/OR/NOT, wildcards, and more)

### Search Pages and Widgets <a href="#search-pages-and-widgets" id="search-pages-and-widgets"></a>

The default search page (`localhost:8080/search`) has a practical set of search widgets. It is customizable and based on the global search page template (also customizable).

![The search page template is useful.](https://resources.learn.liferay.com/images/dxp/latest/en/using-search/getting-started/search-overview/images/05.png)

To add search widgets to a page, click the Add button on the page, select _Widgets_, and open the Search category:

![There are plenty of search widgets.](https://resources.learn.liferay.com/images/dxp/latest/en/using-search/getting-started/search-overview/images/07.png)

To learn more, visit Configuring Search Pages or Search Pages and Widgets.

### Search Configuration and Administration <a href="#search-configuration-and-administration" id="search-configuration-and-administration"></a>

In the Control Panel, you can configure search functionality, view connections and field mappings, and perform search index actions.

You can configure search at the system scope in the Control Panel. In the Configuration category, select _System Settings_ → _Search_. The search settings page appears.

![Search is highly configurable.](https://resources.learn.liferay.com/images/dxp/latest/en/using-search/getting-started/search-overview/images/06.png)

You can examine search connections and field mappings, and execute indexes in the search administration screens. Navigate them in the Control Panel, by selecting _Search_ in the Configuration category.

![The search admin screens are informative and useful.](https://resources.learn.liferay.com/images/dxp/latest/en/using-search/getting-started/search-overview/images/08.png)

Learn more at Search Administration and Tuning.

### Custom Development in Search <a href="#custom-development-in-search" id="custom-development-in-search"></a>

Search customization usually involves at least one of these search phases:

**Indexing** is the sending one or more documents to the search engine. The document contains fields of various types (text, keyword, etc.). The search engine processes each field and determines whether to store the field or analyze it.

**Searching** is sending a search query and obtaining results (a.k.a. hits) from the search engine. Queries and filters can be part of the search request, both of which specify a field to search within and the value to match against. The search engine iterates through each field within the nested queries and filters, and optionally performs special analysis prior to executing the query (search time analysis). Search time analysis can be configured for each field via the mapping definitions.

Search functionality can be extended and invoked using the Service Provider Interfaces and APIs, respectively.

* Service Provider Interfaces (SPIs) are meant to be implemented. In the source code, these are found in modules ending in `-spi` (for example, the `portal-search-spi` module).
* APIs contain methods you can call in your own code. In the source code, these are found in modules ending in `-api` (for example, the `portal-search-api` module).

## Searching for Content <a href="#searching-for-content" id="searching-for-content"></a>

All indexed content (a.k.a. _assets_) can be returned as search results. Custom content can also be indexed, so your installation might have additional asset types beyond the ones included by default. The following image shows an example of the different types of content that is indexed in the portal:

![If these apps are deployed, their content is searchable by default.](https://resources.learn.liferay.com/images/dxp/latest/en/using-search/getting-started/searching-for-content/images/08.png)

### Using the Search Bar <a href="#using-the-search-bar" id="using-the-search-bar"></a>

Using the default portal setup, users can engage with search by entering search terms in the Search Bar. Execute a search and you’re taken to a search page with various search widgets deployed.

![The default search configuration displays a search bar in its default view, beckoning users to enter the search context.](https://resources.learn.liferay.com/images/dxp/latest/en/using-search/getting-started/searching-for-content/images/01.png)

#### Entering Search Terms <a href="#entering-search-terms" id="entering-search-terms"></a>

The portal’s search infrastructure supports full text search as implemented by its supported search engines (Elasticsearch and Solr).

Full text search compares all the words entered in a search query (for example, _stock market_) to all the words in each indexed document. A search engine like Elasticsearch calculates relevance scores to ensure the best results are returned first (like a Blogs Entry about a recent bull market). Anything with words like _stock_ or _market_ is returned.

In addition to full text search, advanced search syntax is supported. The underlying search engine determines the precise behavior, so consult the [Elasticsearch](https://www.elastic.co/guide/en/elasticsearch/reference/7.6/query-dsl-simple-query-string-query.html) or [Solr](https://lucene.apache.org/solr/guide/7\_0/query-syntax-and-parsing.html) documentation for the details.

![Search for text in a specific field using Elasticsearch's Query String syntax.](https://resources.learn.liferay.com/images/dxp/latest/en/using-search/getting-started/searching-for-content/images/02.png)

#### Search Bar Suggestions: Search as the User Types <a href="#search-bar-suggestions-search-as-the-user-types" id="search-bar-suggestions-search-as-the-user-types"></a>

7.4 +

Out of the box on the most recent version 7.4, the Search bar provides search suggestions as users type their search phrases:

![Search Bar Suggestions returns preliminary results as the user types a search phrase into the Search Bar.](https://resources.learn.liferay.com/images/dxp/latest/en/using-search/getting-started/searching-for-content/images/11.png)

For the details on how it works, see Search Bar Suggestions

#### Quoted Search <a href="#quoted-search" id="quoted-search"></a>

Wrapping search bar queries in quotation marks forces the search engine to produce matches more strictly. The exact behavior depends on the type of field being searched.

**Quoted Full Text Search: Ensuring Match Order**

Placing search bar queries in quotes matches the searched terms to the field only if they appear in the exact same order. The search engine does not match the exact quoted phrase, because analysis still takes place on the indexed content and the search phrase.

Consider a blog entry including this text in its `content` field:

> Bring to the table win-win survival _strategies to ensure_ proactive collaboration.

With the default English analyzer, the search phrase _“strategy that ensures”_ returns this blog entry. The same analysis is performed on the search keywords and the indexed text, accounting for stemming, stop word removal, and other tokenization. Quoting the phrase enforces that the separate tokens in the phrase match with the tokens in the indexed document field, including their relative position to each other.

By contrast, entering the search keywords _“strategies ensure”_ will not return the above blogs entry document, because the relative position of the tokens produced by analysis are different between the search keywords and the indexed content.

![Enclose search terms in quotes to enforce that the token order matches in returned content.](https://resources.learn.liferay.com/images/dxp/latest/en/using-search/getting-started/searching-for-content/images/04.png)

**Quoted Keyword Search: Exact Phrase Matching**

When searching non-analyzed fields (e.g., keyword fields), a quoted search phrase and the field value must match exactly.

In a regular search, both analyzed and non-analyzed fields are present in index documents. The non-analyzed fields produce results only if their content matches the quoted search phrase exactly, while the analyzed fields produce results as long as their tokens match the quoted search phrase’s tokens and they appear in the same relative positions.

#### Prefix Searching <a href="#prefix-searching" id="prefix-searching"></a>

With prefix searching, searching for the term _instrument_ returns documents not only containing the full word, but also variants with _instrument_ as the prefix. For example, results with _instruments_, _instrumental_, and _instrumentation_ are also returned.

![Searching for lever also returns leverage and leveraging.](https://resources.learn.liferay.com/images/dxp/latest/en/using-search/getting-started/searching-for-content/images/03.png)

NOTE

Prefix searching is available for many fields out of the box, but it’s more complicated under the hood. The details of the field mapping, including the analyzer used on the field and any transformations performed, determine the final behavior.

Another way to ensure users see results is through search suggestions.

#### Configuring the Search Bar <a href="#configuring-the-search-bar" id="configuring-the-search-bar"></a>

The Search Bar’s behavior can be configured via its widget configuration screen. Configuration options are categorized as Display Settings, Suggestions Configuration, or Advanced Configuration.

![Configure the search bar behavior in its configuration screen.](https://resources.learn.liferay.com/images/dxp/latest/en/using-search/getting-started/searching-for-content/images/05.png)

NOTE

When you configure the globally embedded Search Bar widget at the top of one page, it configures the page-top Search Bar widget on all pages in the site. It also overrides the destination Search Page’s Search Bar widget, if they’re configured differently. However, it does not override Search Bar widgets manually placed on other pages.

**Display Settings**

**Display Template:** Select the widget template that styles the search bar. There’s one alternative template available out-of-the-box: Left Aligned Icon Layout. If using search bar suggestions, you must use the default display template.

**Keywords Parameter Name:** Edit the parameter name for the keywords entered in the search. For example, the default URL when searching for the keyword term _data_ looks like this:

```log
http://localhost:8080/web/guest/search?q=data
```

If you change the Keywords Parameter Name to _keyword_ it looks like this:

```log
http://localhost:8080/web/guest/search?keyword=data
```

**Scope:** Choose between three options: This Site (default), Everything, and Let the User Choose. _This Site_ means only the assets associated with the site where the search is executed are searched. Expand the scope of the search to all sites by selecting _Everything_. To let users choose which scope they want to search, select _Let the User Choose_.

![Let the user choose which scope the search is executed for.](https://resources.learn.liferay.com/images/dxp/latest/en/using-search/getting-started/searching-for-content/images/06.png)

**Scope Parameter Name:** Set the URL parameter name for the scope where the search is taking place. This parameter only appears in the URL if the scope _Let the User Choose_ is selected. The default value is _scope_, so searching for the word _data_ produces the default URL of

```log
http://localhost:8080/web/guest/search?q=data&scope=this-site
```

Changing _scope_ to _target_ would produce this URL:

```log
http://localhost:8080/web/guest/search?q=data&target=this-site
```

**Destination Page:** Provide a friendly URL to the search page. If not configured or if it points to a page that doesn’t exist, a message appears for administrators that the search bar must be configured for it to appear to users.

**Suggestions Configuration**

If Search Bar Suggestions is available and enabled, additional configurations appear in the Search Bar widget configuration. See Configuring Search Bar Suggestions to understand these configuration options.

**Advanced Configuration**

**Use Advanced Search Syntax:** If using Elasticsearch, enabling this allows users to enter Query String Syntax into the Search Bar. If using Solr, consult its documentation for the proper syntax.

**Show Results from Staged Sites:** Content from Staging environments is not searchable on live sites, regardless of this setting. This setting enables searching for content from other local staging sites when the Everything scope is configured in a site with local staging enabled.

### Searching for Objects <a href="#searching-for-liferay-objects" id="searching-for-liferay-objects"></a>

Version 7.4 includes Objects, a GUI-driven application-building framework. Fields of each Object’s definition can be declared searchable; Object Entries (the records submitted by the application’s users) can then be queried and returned in the Search Results widget.

IMPORTANT

The scope of the Object determines how it must be searched:

* If the scope of the Object is Company, the Object’s Entries are only searched when the Search Bar is configured to search Everything.
* If the scope of the Object is Site, the Object’s Entries can be searched when the Search bar is configured to search in This Site.

The Type Facet and the Search Results widget support searching for Objects in the following manner:

*   The Type Facet automatically includes published Objects:

    ![Published Objects are added to the Type Facet widget.](https://resources.learn.liferay.com/images/dxp/latest/en/using-search/getting-started/searching-for-content/images/09.png)
*   The Search Results widget automatically displays the Object Entry’s title and a summary.

    ![A title and a content summary are displayed for Object Entries.](https://resources.learn.liferay.com/images/dxp/latest/en/using-search/getting-started/searching-for-content/images/10.png)

For version 7.4 +, clicking an object entry in search results redirects to the entry’s [display page](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/displaying-object-entries#creating-display-page-templates-for-objects), if one exists.

For versions 2024.Q1+, the entries of account restricted objects appear in search results, if the search user has the `Accounts > Account Entry: View` permission in an account- or regular-scoped role. Because many permissions clauses are added to the search query for users with administrative access to sites, organizations, and accounts, poor search performance can result. If you notice this in your system, you can limit the number of permissions clauses in the search framework’s initial permissions check. To do this, configure the Permission Checker’s Permissions Term Limit setting.

\
