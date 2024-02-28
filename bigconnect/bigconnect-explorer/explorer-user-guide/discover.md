# Discover

This is where you search through data stored in BigConnect. BigConnect has ElasticSearch natively built-in to support all types of queries and aggregations supported by ElasticSearch.&#x20;

Some limitations do apply if security is enabled, because it changes the way data is stored. This means that not all ElasticSearch aggregations are supported when security is enabled.

![](<../../../.gitbook/assets/image (485).png>)

In the top part you have a global search field where you can enter search terms. Have a look at the **Querying data** wizard on the search page to get some hints on what is the search syntax.

To quickly summarize, the following search operators are supported in the search field:

* **Wildcards**: _qu?ck bro\*_
* **Fuzzy**: _quikc\~ brwn\~ foks\~_
* **Proximity** **searches**: _"fox quick"\~5_
* **Term Boosting**: _quick^2 fox_
* **Boolean operators**: _AND, OR, NOT_
* **Term Grouping**: _(quick OR brown) AND fox_

On the left side of the search page you have the **Filters**. Here you can choose what you want to search for (Entities or Relationships, the properties to filter on and sort conditions). These work  on new searches and for filtering existing searches.

To see what's available in the system, type \* in the search field and hit Enter:

![](<../../../.gitbook/assets/image (509).png>)

We have a new panel on the right side called **Refinements**. By default, BigConnect aggregates all search results by some predefined system properties: Concept and Media Tag in our case. You can drill down in search results by clicking on a concept or media tag.

For example, if we want to see all objects that have the Media Tag = animal, click on the animal media tag and the search results will be filtered accordingly.

![](<../../../.gitbook/assets/image (443).png>)

You can further drill down on available Refinements or use Filters to do so.

## Filtering

Search results can be filtered by:

* **Concepts**:  add one or more concepts&#x20;
* **Relation**: add one or more types of relations
* **Property**: add one or more properties&#x20;

Filtering by Properties depends on the type of the property being selected. Different operators are available based on the type of property, as follows:

* **String**:&#x20;
  * _CONTAINS WORD_: the Property value must contain the specified word
  * _EQUALS_: the Property value must exactly have the specified value
  * _HAS `Property`_: the Property is set
  * _DOES NOT HAVE `Property`_: the Property is not set
* **Numeric**:&#x20;
  * _LESS THAN, GREATHER THAN_: the Property value must be strictly bigger or smaller than the specified value (equality is not considered)
  * _LESS THAN EQUAL, GREATHER THAN EQUAL_: the Property value must be bigger or smaller than the specified value (equality is considered)
  * EQUALS: the Property value must equal the specified value
  * _HAS `Property`_: the Property is set
  * _DOES NOT HAVE `Property`_: the Property is not set
* **Date**:&#x20;
  * _BEFORE, AFTER_: the Property value must be strictly before or after the specified date (equality is not considered)
  * _BEFORE EQUAL, AFTER EQUAL_: the Property value must be before or after the specified value (equality is considered)
  * _BETWEEN_: the Property value must be between the provided dates
  * _EQUALS_: the Property value must equal the specified value
  * _HAS `Property`_: the Property is set
  * _DOES NOT HAVE `Property`_: the Property is not set

Multiple property filters can be applied. By default, the operator applied between subsequent filters is AND. However, you can change this to OR or NOT.

You can also group property filters together using the grouping icons: <img src="../../../.gitbook/assets/image (573).png" alt="" data-size="original">&#x20;

## Spatial Filtering

A special property named `Geolocation` exists on all objects.  This property can have latitude and longitude information in the WGS 84 EPSG format. BigConnet Explorer can search objects that have their Geolocation in a specific range:

* LATITUDE, LONGITUDE: The location latitude and longitude must equal the provided values
* KM: radius in km of a circle with center (LATITUDE, LONGITUDE) to search for objects

The spatial search can also be performed visually, by drawing a circle on the map around the area you want to search.

<figure><img src="../../../.gitbook/assets/image (1069).png" alt=""><figcaption></figcaption></figure>

## Advanced filtering

### Terms

A query is broken up into terms and operators. There are two types of terms: Single Terms and Phrases.

A Single Term is a single word such as "test" or "hello".

A Phrase is a group of words surrounded by double quotes such as "hello dolly".

Multiple terms can be combined together with Boolean operators to form a more complex query (see below).

### Fields

Lucene supports fielded data, which Search Lucene API modules often use in faceted searches. By default, Search Lucene API searches the _contents_ field. However, you can search data in a specific field by typing the field name followed by a colon ":" and the term you are looking for. For example, if you to search for a node entitled "The Right Way" which contains the text "go", you can enter:

`title:"The Right Way" AND contents:go`\
or\
`title:"The Right Way" AND go`

Since _contents_ is the default field, the field indicator is not required. Note: The field is only valid for the term that it directly precedes, so the query

`title:Right Way`

Will only find "Right" in the title field. It will attempt to find "way" in the default field (in this case the _contents_ field).

### Wildcard Searches

Lucene supports single and multiple character wildcard searches within single terms (not within phrase queries).

To perform a single character wildcard search use the "?" symbol.

To perform a multiple character wildcard search use the "\*" symbol.

The single character wildcard search looks for terms that match that with the single character replaced. For example, to search for "text" or "test" you can use the search:

`te?t`

Multiple character wildcard searches looks for 0 or more characters. For example, to search for test, tests or tester, you can use the search:

`test*`

You can also use the wildcard searches in the middle of a term.

`te*t`\
Note: You cannot use a \* or ? symbol as the first character of a search, unless you set the Minimum wildcard prefix on /admin/settings/luceneapi\_node/performance to No minimum.

### Fuzzy Searches

Lucene supports fuzzy searches based on the Levenshtein Distance, or Edit Distance algorithm. To do a fuzzy search use the tilde, "\~", symbol at the end of a Single word Term. For example to search for a term similar in spelling to "roam" use the fuzzy search:

`roam~`

This search will find terms like foam and roams. An additional parameter can specify the required similarity. The value is between 0 and 1,, and with a value closer to 1, only terms with a higher similarity will be matched. For example:

`roam~0.8`

The default value is 0.5 if the parameter is not given.

### Proximity Searches

Lucene supports finding words are a within a specific distance away. To do a proximity search use the tilde, "\~", symbol at the end of a Phrase. For example, to search for "drupal" and "lucene" within 10 words of each other, enter

`"drupal lucene"~10`

### Range Searches

Range Queries allow one to match documents whose field(s) values are between the lower and upper bound specified by the Range Query. Range Queries can be inclusive or exclusive of the upper and lower bounds. Sorting is done lexicographically.

`created:[1234567000 TO 1234568000]`

This will find documents whose _created_ fields have values between 1234567000 and 1234568000, inclusive. Note that Range Queries are not reserved for date fields. You could also use range queries with non-date fields:

`title:{Drupal TO Joomla}`

This will find all documents whose titles are between Drupal and Joomla, but not including Drupal and Joomla. Inclusive range queries are denoted by square brackets. Exclusive range queries are denoted by curly brackets.

### Boosting a Term

Lucene provides the relevance level of matching documents based on the terms found. To boost a term use the caret, "^", symbol with a boost factor (a number) at the end of the term you are searching. The higher the boost factor, the more relevant the term will be.

Boosting allows you to control the relevance of a document by boosting its term. For example, if you are searching for

`lucene drupal`

and you want the term "lucene" to be more relevant, boost it using the ^ symbol along with the boost factor next to the term. You would type:

`lucene^4 drupal`

This will make documents with the term lucene appear more relevant. You can also boost Phrase Terms as in the example:

`"search lucene API"^4 drupal`

By default, the boost factor is 1. Although the boost factor must be positive, it can be less than 1 (e.g. 0.2).

### Boolean Operators

Boolean operators allow terms to be combined through logic operators. Lucene supports AND, "+", OR, AND NOT, and "-" as Boolean operators (Note: Boolean operators must be ALL CAPS).

#### OR

The OR operator is the default conjunction operator. This means that if there is no Boolean operator between two terms, the OR operator is used. The OR operator links two terms and finds a matching document if either of the terms exist in a document. This is equivalent to a union using sets. The symbol || can be used in place of the word OR.

To search for documents that contain either "search lucene" or just "lucene" use the query:

`"search lucene" lucene`\
or\
`"search lucene" OR lucene`

#### AND

The AND operator matches documents where both terms exist anywhere in the text of a single document. This is equivalent to an intersection using sets. The symbol && can be used in place of the word AND. To search for documents that contain "drupal" and "lucene" use the query:

`drupal AND Lucene`

#### +

The "+" or required operator requires that the term after the "+" symbol exist somewhere in a the field of a single document. To search for documents that must contain "drupal" and may contain "lucene" use the query:

`+drupal lucene`

#### AND NOT

The AND NOT operator excludes documents that contain the term after AND NOT. This is equivalent to a difference using sets. The symbol "!" can be used in place of the word AND NOT.

To search for documents that contain "search lucene" but not "drupal" use the query:

`"search lucene" AND NOT "drupal"`

Note: The AND NOT operator cannot be used with just one term. For example, the following search will return no results:

`AND NOT "jakarta apache"`

#### -

The "-" or prohibit operator excludes documents that contain the term after the "-" symbol. To search for documents that contain "drupal" but not "search lucene" use the query:

`drupal -"search lucene"`

### Grouping

Lucene supports using parentheses to group clauses to form sub queries. This can be very useful if you want to control the boolean logic for a query. To search for either "drupal" or "wordpress" and "lucene" use the query:

`(drupal OR wordpress) AND lucene`

This eliminates any confusion and makes sure you are searching for lucene in either sites about drupal or wordpress.

### Field Grouping

Lucene supports using parentheses to group multiple clauses to a single field. To search for a title that contains both the word "return" and the phrase "pink panther" use the query:

`title:(+return +"pink panther")`

### Escaping Special Characters

Lucene supports escaping special characters that are part of the query syntax. The current list special characters are + - && || ! ( ) { } \[ ] ^ " \~ \* ? : \\

To escape these character use the \ before the character. For example to search for (1+1):2 use the query:

`\(1\+1\)\:2`

### Casing and diacritics

By default, BigConnect Explorer performs searches in a case-insensitive manner. This is a configurable  options and must be enforced before the Elasticsearch index is created. It cannot be changed after the index was created.&#x20;

Diacritics are reduced to normal characters as much as possible. Lucene converts alphabetic, numeric, and symbolic Unicode characters which are not in the first 127 ASCII characters (the "Basic Latin" Unicode block) into their ASCII equivalents, if one exists.

## Sorting

Search results can be sorted by one or more properties. Select the desired properties from the dropdown and click on them afterwards to change the sort order.

## Pagination

Because a search can yield millions of results, search results are always paginated. The total number of results and number of pages are fetched eagerly and the user can browse results by navigating pages. Users can configure the number of search results displayed on a page.

## Saved searches

You can save the current query, its filters, refinements and sort properties by clicking the Saved Searches dropdown:

![](<../../../.gitbook/assets/image (490).png>)

Just enter a name for your saved search and choose whether you want it to be available to all users (Global). By default, a saved search is available only to the user that created it.

{% hint style="info" %}
Saved Searches are used in the Saved Search dashboard widget and in tools like Graph, Map and Charts to load multiple items from the graph store.
{% endhint %}

### Supported aggregations

Saved searches can be aggregated using the **Saved Search** dashboard widget. The following aggregation are supported:

* [Terms Aggregation](https://www.elastic.co/guide/en/elasticsearch/reference/6.8/search-aggregations-bucket-terms-aggregation.html)
* [Significant Terms Aggregation](https://www.elastic.co/guide/en/elasticsearch/reference/6.8/search-aggregations-bucket-significantterms-aggregation.html)
* [Date Histogram](https://www.elastic.co/guide/en/elasticsearch/reference/6.8/search-aggregations-bucket-datehistogram-aggregation.html)
* [GeoHash grid Aggregation](https://www.elastic.co/guide/en/elasticsearch/reference/6.8/search-aggregations-bucket-geohashgrid-aggregation.html)
* [Histogram Aggregation](https://www.elastic.co/guide/en/elasticsearch/reference/6.8/search-aggregations-bucket-histogram-aggregation.html)
* [Nested Aggregation](https://www.elastic.co/guide/en/elasticsearch/reference/6.8/search-aggregations-bucket-nested-aggregation.html)
* [Range Aggregation](https://www.elastic.co/guide/en/elasticsearch/reference/6.8/search-aggregations-bucket-range-aggregation.html)
* [IP Range Aggregation](https://www.elastic.co/guide/en/elasticsearch/reference/6.8/search-aggregations-bucket-iprange-aggregation.html)
* [Missing Aggregation](https://www.elastic.co/guide/en/elasticsearch/reference/6.8/search-aggregations-bucket-missing-aggregation.html)

## Exporting search results

You can export all search results or only selected ones. The top export icon next to the Saved Searches dropdown <img src="../../../.gitbook/assets/image (572).png" alt="" data-size="original"> is used to export all search results.&#x20;

![](<../../../.gitbook/assets/image (487).png>)

If you want to export only certain search results, select the rows you want to export and click the export dropdown next to the pagination options. &#x20;

![](<../../../.gitbook/assets/image (525).png>)
