# Searching for Assets

As explained in the Search introduction, all indexed assets can be returned as search results. Developers can create their own assets, so your installation might have additional asset types beyond the ones included by default.

**Searching for Users:** When you click an asset in the search results, it’s displayed in an Asset Publisher (unless the _View in Context_ option is selected in the Search Results portlet). Users are different, though. Think of them as invisible assets, not intended for display in the Asset Publisher application. While Users appear as search results with other indexed assets, when you click one you’re taken to the User’s profile page. If public personal pages are disabled, clicking on a User from the list of search results shows you a blank page.

### Search Bar <a href="#search-bar" id="search-bar"></a>

Users enter the search context in the search bar. Users enter search terms, hit the _Enter_ button (or click the magnifying glass icon), and they’re taken to a search page with various search widgets deployed.

If using the Search Bar in the legacy search portlet, users see a maximized view of the search portlet displaying any results and facets that apply.&#x20;

![Figure 1: The default search configuration displays a search bar in its default view, beckoning users to enter the search context.](https://liferay-support.zendesk.com/hc/article\_attachments/360036302472/search-bar.png)

#### ENTERING SEARCH TERMS <a href="#entering-search-terms" id="entering-search-terms"></a>

The portal’s search infrastructure supports full text search as implemented by its supported search engines ([Elasticsearch](https://www.elastic.co/guide/en/elasticsearch/reference/7.x/full-text-queries.html) and [Solr](http://lucene.apache.org/solr/features.html)).

Full text search compares all the words entered in a search query (for example, _space vacation_) to all the words in each index document. A search engine like Elasticsearch calculates relevance scores to ensure the best results are returned first (like a Blogs Entry titled _Is a vacation in space right for you?_) and lots of matching results are returned (anything with either the word _vacation_ or _space_ is returned).

In addition to full text search, advanced search syntax is supported. MWARE Portal relies on the underlying search engine for this behavior, so consult the [Elasticsearch](https://www.elastic.co/guide/en/elasticsearch/reference/7.x/query-dsl-query-string-query.html#query-string-syntax) or [Solr](https://lucene.apache.org/solr/guide/6\_6/query-syntax-and-parsing.html) documentation for the details.

![Figure 2: Search for text in a specific field using Elasticsearchs Query String syntax.](https://liferay-support.zendesk.com/hc/article\_attachments/360036302412/search-advanced-syntax.png)

#### MATCHING EXACT PHRASES: QUOTED SEARCH <a href="#matching-exact-phrases-quoted-search" id="matching-exact-phrases-quoted-search"></a>

What if users want their search terms (for example, _agile frameworks_) to produce only results with the exact phrase, as typed? In a regular full text search, searching _agile frameworks_ returns search results containing just the terms _agile_ and _frameworks_, and hits containing both terms but separated by other text, as well as results with the exact phrase match. To ensure that only hits with the exact phrase are returned, enclose it in quotes: _“agile frameworks”_.

![Figure 3: Search for exact phrase matches by enclosing search terms in quotes. If a user searched for agile frameworks, this result would not be returned.](https://liferay-support.zendesk.com/hc/article\_attachments/360036302432/search-quoted.png)

#### PREFIX SEARCHING <a href="#prefix-searching" id="prefix-searching"></a>

If you’re searching in a site for classical musicians, you might search for the term _instrument_. This search returns documents with the full word in them, but it also returns variants with _instrument_ as the prefix. For example, results with _instruments_, _instrumental_, and _instrumentation_ are also returned.

![Figure 4: Searching for lever also returns leverage and leveraging.](https://liferay-support.zendesk.com/hc/article\_attachments/360036302452/search-prefix.png)

**Note:** Prefix searching is available for many fields out of the box, but as with most things related to search behavior, it’s more complicated under the hood. The details of the field mapping, including the analyzer used on the field and any transformations performed, determine the final behavior.

Another way to ensure users see results is through search suggestions.

#### CONFIGURING THE SEARCH BAR <a href="#configuring-the-search-bar" id="configuring-the-search-bar"></a>

Configure the Search Bar’s behavior via its portlet configuration screen.

![Figure 5: Configure the search bar behavior in its configuration screen.](https://liferay-support.zendesk.com/hc/article\_attachments/360036301451/search-bar-configuration.png)

**Note:** When you configure the globally embedded Search Bar widget at the top of one page, it configures the page-top Search Bar widget on all pages in the site. It also overrides the destination Search Page’s Search Bar portlet, if they’re configured differently. However, it does not override Search Bar widgets manually placed on other pages.

There are several options:

**Keywords Parameter Name**Edit the parameter name for the keywords entered in the search. For example, the default URL when searching for the keyword term _data_ looks like this:

http://localhost:8080/web/guest/search?q=data

If you change the Keywords Parameter Name to _keyword_ it looks like this:

```
http://localhost:8080/web/guest/search?keyword=data
```

**Scope**Choose between three options: This Site (default), Everything, and Let the User Choose. _This Site_ means only the assets associated with the site where the search is executed are searched. Expand the scope of the search to all sites by selecting _Everything_. To let users choose which scope they want to search, select _Let the User Choose_.

![Figure 6: Let the user choose which scope the search is executed for.](https://liferay-support.zendesk.com/hc/article\_attachments/360036301471/search-scope.png)

**Scope Parameter Name** : Set the URL parameter name for the scope where the search is taking place. This parameter only appears in the URL if the scope _Let the User Choose_ is selected. The default value is _scope_, so searching for the word _data_ produces the default URL of

```
http://localhost:8080/web/guest/search?q=data&scope=this-site
```

Changing _scope_ to _target_ would produce this URL:

```
http://localhost:8080/web/guest/search?q=data&target=this-site
```

**Destination Page**\
Provide a friendly URL to the search page. If not configured or if it points to a page that doesn’t exist, a message appears for administrators that the search bar must be configured for it to appear to users.

**Use Advanced Search Syntax**\
If using Elasticsearch, enabling this allows users to enter [Query String Syntax](https://www.elastic.co/guide/en/elasticsearch/reference/7.x/query-dsl-query-string-query.html#query-string-syntax) into the Search Bar. If using Solr, consult its documentation for the [proper syntax](https://lucene.apache.org/solr/guide/6\_6/query-syntax-and-parsing.html).

### Search Suggestions <a href="#search-suggestions" id="search-suggestions"></a>

Suggest search terms to users when their initial queries are suboptimal. Spell check settings allow administrators to configure the Search application so that if a user types a search term that doesn’t return many results (for example, a slightly misspelled werd), the user can be prompted to improve the search.

To configure the spell check settings,

1. You must first reindex the spell check indexes. Go to _Control Panel_ → _Configuration_ → _Search_, then click _Execute_ next to _Reindex all spell check indexes_.
2. Add the Suggestions widget to the search page.
3. Open its configuration screen. Click the widget Options button (![Options](https://liferay-support.zendesk.com/hc/article\_attachments/360036301491/icon-app-options.png)) and select _Configuration_.

![Figure 7: Configure the suggestion settings to allow for user input mistakes and help lead users to results.](https://liferay-support.zendesk.com/hc/article\_attachments/360036302492/search-suggestions.png)

There are three main settings here:

\*\*Display “Did you mean…” if the number of search results does not meet thethreshold.\*\*Present users alternate, spell checked search queries if their search did not return a minimum number of results (50 by default).

**Display Related Queries**

If the number of search results doesn’t meet the specified threshold (50 by default), display up to a maximum number of alternative queries (10 by default).

**Add New Related Queries Based on Successful Queries**

Index a user’s search query if it produces a minimum number of results (50 by default), so it can be displayed to users as a suggestion. If the Display Related Queries setting is enabled, it’s used as a related query for similar search queries that don’t produce enough results.
