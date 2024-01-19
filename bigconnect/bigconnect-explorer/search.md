# Search

BigGraph has ElasticSearch natively built. Thus it supports all types of queries and aggregations supported by ElasticSearch. 

Some limitations do apply if security is enabled, because it changes the way data is stored. This means that not all ElasticSearch aggregations are supported when security is enabled.

## Lucene query language

### Terms

A query is broken up into terms and operators. There are two types of terms: Single Terms and Phrases.

A Single Term is a single word such as "test" or "hello".

A Phrase is a group of words surrounded by double quotes such as "hello dolly".

Multiple terms can be combined together with Boolean operators to form a more complex query \(see below\).

### Fields

Lucene supports fielded data, which Search Lucene API modules often use in faceted searches. By default, Search Lucene API searches the _contents_ field. However, you can search data in a specific field by typing the field name followed by a colon ":" and the term you are looking for. For example, if you to search for a node entitled "The Right Way" which contains the text "go", you can enter:

`title:"The Right Way" AND contents:go`  
or  
`title:"The Right Way" AND go`

Since _contents_ is the default field, the field indicator is not required. Note: The field is only valid for the term that it directly precedes, so the query

`title:Right Way`

Will only find "Right" in the title field. It will attempt to find "way" in the default field \(in this case the _contents_ field\).

#### Wildcard Searches

Lucene supports single and multiple character wildcard searches within single terms \(not within phrase queries\).

To perform a single character wildcard search use the "?" symbol.

To perform a multiple character wildcard search use the "\*" symbol.

The single character wildcard search looks for terms that match that with the single character replaced. For example, to search for "text" or "test" you can use the search:

`te?t`

Multiple character wildcard searches looks for 0 or more characters. For example, to search for test, tests or tester, you can use the search:

`test*`

You can also use the wildcard searches in the middle of a term.

`te*t`  
Note: You cannot use a \* or ? symbol as the first character of a search, unless you set the Minimum wildcard prefix on /admin/settings/luceneapi\_node/performance to No minimum.

#### Fuzzy Searches

Lucene supports fuzzy searches based on the Levenshtein Distance, or Edit Distance algorithm. To do a fuzzy search use the tilde, "~", symbol at the end of a Single word Term. For example to search for a term similar in spelling to "roam" use the fuzzy search:

`roam~`

This search will find terms like foam and roams. An additional parameter can specify the required similarity. The value is between 0 and 1,, and with a value closer to 1, only terms with a higher similarity will be matched. For example:

`roam~0.8`

The default value is 0.5 if the parameter is not given.

#### Proximity Searches

Lucene supports finding words are a within a specific distance away. To do a proximity search use the tilde, "~", symbol at the end of a Phrase. For example, to search for "drupal" and "lucene" within 10 words of each other, enter

`"drupal lucene"~10`

#### Range Searches

Range Queries allow one to match documents whose field\(s\) values are between the lower and upper bound specified by the Range Query. Range Queries can be inclusive or exclusive of the upper and lower bounds. Sorting is done lexicographically.

`created:[1234567000 TO 1234568000]`

This will find documents whose _created_ fields have values between 1234567000 and 1234568000, inclusive. Note that Range Queries are not reserved for date fields. You could also use range queries with non-date fields:

`title:{Drupal TO Joomla}`

This will find all documents whose titles are between Drupal and Joomla, but not including Drupal and Joomla. Inclusive range queries are denoted by square brackets. Exclusive range queries are denoted by curly brackets.

#### Boosting a Term

Lucene provides the relevance level of matching documents based on the terms found. To boost a term use the caret, "^", symbol with a boost factor \(a number\) at the end of the term you are searching. The higher the boost factor, the more relevant the term will be.

Boosting allows you to control the relevance of a document by boosting its term. For example, if you are searching for

`lucene drupal`

and you want the term "lucene" to be more relevant, boost it using the ^ symbol along with the boost factor next to the term. You would type:

`lucene^4 drupal`

This will make documents with the term lucene appear more relevant. You can also boost Phrase Terms as in the example:

`"search lucene API"^4 drupal`

By default, the boost factor is 1. Although the boost factor must be positive, it can be less than 1 \(e.g. 0.2\).

### Boolean Operators

Boolean operators allow terms to be combined through logic operators. Lucene supports AND, "+", OR, AND NOT, and "-" as Boolean operators \(Note: Boolean operators must be ALL CAPS\).

#### OR

The OR operator is the default conjunction operator. This means that if there is no Boolean operator between two terms, the OR operator is used. The OR operator links two terms and finds a matching document if either of the terms exist in a document. This is equivalent to a union using sets. The symbol \|\| can be used in place of the word OR.

To search for documents that contain either "search lucene" or just "lucene" use the query:

`"search lucene" lucene`  
or  
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

Lucene supports escaping special characters that are part of the query syntax. The current list special characters are + - && \|\| ! \( \) { } \[ \] ^ " ~ \* ? : \

To escape these character use the \ before the character. For example to search for \(1+1\):2 use the query:

`\(1\+1\)\:2`

## Supported aggregations

When security is enabled, the following aggregation are fully supported:

* [Terms Aggregation](https://www.elastic.co/guide/en/elasticsearch/reference/6.8/search-aggregations-bucket-terms-aggregation.html)
* [Significant Terms Aggregation](https://www.elastic.co/guide/en/elasticsearch/reference/6.8/search-aggregations-bucket-significantterms-aggregation.html)
* [Date Histogram](https://www.elastic.co/guide/en/elasticsearch/reference/6.8/search-aggregations-bucket-datehistogram-aggregation.html)
* [GeoHash grid Aggregation](https://www.elastic.co/guide/en/elasticsearch/reference/6.8/search-aggregations-bucket-geohashgrid-aggregation.html)
* [Histogram Aggregation](https://www.elastic.co/guide/en/elasticsearch/reference/6.8/search-aggregations-bucket-histogram-aggregation.html)
* [Nested Aggregation](https://www.elastic.co/guide/en/elasticsearch/reference/6.8/search-aggregations-bucket-nested-aggregation.html)
* [Range Aggregation](https://www.elastic.co/guide/en/elasticsearch/reference/6.8/search-aggregations-bucket-range-aggregation.html)
* [IP Range Aggregation](https://www.elastic.co/guide/en/elasticsearch/reference/6.8/search-aggregations-bucket-iprange-aggregation.html)
* [Missing Aggregation](https://www.elastic.co/guide/en/elasticsearch/reference/6.8/search-aggregations-bucket-missing-aggregation.html)

