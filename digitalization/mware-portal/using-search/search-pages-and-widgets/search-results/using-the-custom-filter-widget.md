# Using the Custom Filter Widget

With Custom Filters, you can contribute queries to the main search query, filtering search results to control what’s returned in the Search Results widget. You can make the filter widgets visible or invisible and decide if they’re changeable or immutable.

![Apply a custom filter to weed out certain search results.](https://resources.learn.liferay.com/images/dxp/latest/en/using-search/search-pages-and-widgets/search-results/using-the-custom-filter-widget/images/03.png)

There are many use cases satisfied by incorporating Custom Filters into your search page.

For now, consider these basic usages:

| Use Case                                                                                                                                   | `Configuration` _(Value)_                                                                                                                                                                                 |
| ------------------------------------------------------------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **User Title Filter:** End Users directly manipulate the filter key to refine results to the one they were looking for, using the Title    | <p><code>Filter Field</code> <em>(title_en_US)</em><br><code>Filter Value</code> <em>(podcast)</em><br><code>Filter Query Type</code> <em>(Match)</em><br><code>Occur Type</code> <em>(must_not)</em></p> |
| **Hard-Coded Filtering:** An administrative User configures the Search Page with filters that are invisible and unchangeable for end Users | <p><em><strong>Add to the above configuration</strong></em><br><code>Invisible</code> <em>(True–checked)</em><br><code>Immutable</code> <em>(True–checked)</em></p>                                       |

Custom filters can do so many things, it’s impossible to list them all. What follows is a widget configuration tour. You can see some example configurations in [Custom Filter Examples](https://learn.liferay.com/web/guest/w/dxp/using-search/search-pages-and-widgets/search-results/custom-filter-examples).

### [Adding a Custom Filter](https://learn.liferay.com/web/guest/w/dxp/using-search/search-pages-and-widgets/search-results/using-the-custom-filter-widget#adding-a-custom-filter) <a href="#adding-a-custom-filter" id="adding-a-custom-filter"></a>

To explore all the options you have with the Custom Filter widget, you need one on the page:

1. Open the Add menu (![Add](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add-widget.png)) for the page and expand the Widgets section.
2. From the Search section, drag a Custom Filter onto the page.

![A custom filter has no impact until it's configured.](https://resources.learn.liferay.com/images/dxp/latest/en/using-search/search-pages-and-widgets/search-results/using-the-custom-filter-widget/images/01.png)

### [Custom Filter Configuration](https://learn.liferay.com/web/guest/w/dxp/using-search/search-pages-and-widgets/search-results/using-the-custom-filter-widget#custom-filter-configuration) <a href="#custom-filter-configuration" id="custom-filter-configuration"></a>

To begin filtering search results, open the widget’s Options Menu (![Options](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-app-options.png)) and click _Configuration_.

**Filter Field (text):** Most often, filters operate on a specific field. Set the name of the indexed field to be filtered (for example, `title`). You won’t need this if the Filter Query Type is set to a type that doesn’t require a field, such as _Regexp_.

The Query String and Script queries do not require a Filter Field to be set. All other queries require at least one field.

To find the fields present in the Liferay DXP index, use [the Field Mappings UI in the Control Panel](https://learn.liferay.com/web/guest/w/dxp/using-search/search-pages-and-widgets/search-results/using-the-custom-filter-widget#finding-fields).

**Filter Value (text):** For most filters, you must enter a text value here that specifies the text to apply the filter on in the specified field (for example, set a _Match_ query to the text _street_ on the `title_en_US` field). Some Filter Query Types require special notation, as in the case of the [_Regexp_](https://www.elastic.co/guide/en/elasticsearch/reference/8.8/query-dsl-regexp-query.html) query.

**Filter Query Type (select list):** Select the query type to filter results by. Available types include Bool, Date Range, Exists, Fuzzy, Match, Match Phrase, Match Phrase Prefix, Multi Match, Prefix, Query String, Regexp, Script, Simple Query String, Term, Wildcard.

To learn more about these queries, visit the [Elasticsearch documentation](https://www.elastic.co/guide/en/elasticsearch/reference/8.8/query-dsl.html).

**Occur (select list):** Set the occurrence type for the query being contributed to the search. Options include Filter, must, must\_not, and should.

To understand each type, see the [Elasticsearch documentation](https://www.elastic.co/guide/en/elasticsearch/reference/8.8/query-dsl-bool-query.html).

**Query Name (text):** Set the name of the contributed query. This is unnecessary unless this filter acts as a parent query to another filter that contributes child clauses; in that case set this filter’s Query Name as the child filter’s Parent Query Name. This parent/child behavior is only available for filters of type Bool.

**Parent Query Name (text):** When contributing a child clause to a Bool query, set this to match the Query Name configured in the parent Custom Filter widget. Otherwise, leave it blank.

**Boost (number):** [Boost](https://www.elastic.co/guide/en/elasticsearch/reference/8.8/query-dsl-term-query.html#term-field-params) the score of the results matching this query. Specify any whole or decimal number here that makes sense.

If you always want results matching this at the top, set the Boost value really high (e.g., _1000_).

**Custom Heading (text):** Enter the heading to display for this filter. If not set, the Filter Field’s value is displayed.

**Custom Parameter Name (text):** Specify a URL parameter name for the filter. If not set, the Filter Field’s value is used.

**Invisible (boolean):** If checked, the widget is invisible to regular users. The Filter Value from the configuration is applied by default, but users can still filter for other values via URL Parameter. You can prevent URL parameter filtering with the Immutable setting (see below).

**Immutable (boolean):** Enable this to ensure that the Filter Value cannot be changed by regular users. The widget becomes invisible to them _and_ filter values set via URL parameters are not accepted. The Filter Value set in the widget configuration is applied at all times (unless it’s disabled). Note that setting Immutable to true means that the Custom Parameter Name value is ignored.

**Disabled (boolean):** If checked, the query is ignored and doesn’t participate in searches. This gives you a quick way to stop the filter but keep the configuration so it can be re-enabled later.

**Federated Search Key (text):** Enter the key of an alternate Search this widget is participating on. If it’s set, be aware that the default index isn’t searched at all. If not set, this widget participates on the default search.

Values in this field typically match the name of an application-defined index.

### [Finding Fields](https://learn.liferay.com/web/guest/w/dxp/using-search/search-pages-and-widgets/search-results/using-the-custom-filter-widget#finding-fields) <a href="#finding-fields" id="finding-fields"></a>

To find the fields you can filter by in the Custom Filter widget, users with the proper permissions can navigate to _Control Panel_ → _Configuration_ → _Search_. From there, open the Field Mappings tab and browse the mappings for the `liferay-[companyId]` index. Scroll to the [`properties`](https://www.elastic.co/guide/en/elasticsearch/reference/current/properties.html) section of the mapping.

![Browse the field mappings to find Liferay DXP's indexed fields.](https://resources.learn.liferay.com/images/dxp/latest/en/using-search/search-pages-and-widgets/search-results/using-the-custom-filter-widget/images/04.png)

### [Finding and Using Nested Fields](https://learn.liferay.com/web/guest/w/dxp/using-search/search-pages-and-widgets/search-results/using-the-custom-filter-widget#finding-and-using-nested-fields) <a href="#finding-and-using-nested-fields" id="finding-and-using-nested-fields"></a>

Available 7.2 FP10+, 7.3 FP1+, 7.4+

Object definition and [web content structure](https://learn.liferay.com/web/guest/w/dxp/liferay-internals/reference/7-3-breaking-changes#dynamic-data-mapping-fields-in-elasticsearch-have-changed-to-a-nested-document) fields are indexed as nested fields in Elasticsearch. To filter by a nested field, you must add three Custom Filter widgets to the page. The first widget is for adding the [Elasticsearch `nested` query](https://www.elastic.co/guide/en/elasticsearch/reference/8.8/query-dsl-nested-query.html), while the other two are for defining child queries that match the nested field’s name and value. When the final portal query is generated, the `nested` query wraps the child queries.

IMPORTANT

If you require custom filters on multiple nested fields in the same page, you must configure separate parameter names for each child query. See [Custom Filter Examples](https://learn.liferay.com/web/guest/w/dxp/using-search/search-pages-and-widgets/search-results/custom-filter-examples#boosting-matches-to-nested-fields) for an example.

To find nested fields in existing documents in the index, use the [Display Results in Document Form](https://learn.liferay.com/web/guest/w/dxp/using-search/search-pages-and-widgets/search-results/configuring-the-search-results-widget#inspecting-search-engine-documents) setting in the Search Results widget.

For example, an object entry has a `nestedFieldArray` with nested fields:

```json
"nestedFieldArray" : [
   {
     "fieldName" : "lastAcessed",
     "valueFieldName" : "value_date",
     "value_date" : "20230502000000"
   }
],
```

Meanwhile, a web content article has a `ddmFieldArray` with nested fields:

```json
"ddmFieldArray" : [
   {
     "ddmFieldName" : "ddm__keyword__44012__Checkbox08350381_en_US",
     "ddmFieldValueKeyword_en_US" : "true",
     "ddmFieldValueKeyword_en_US_String_sortable" : "true",
     "ddmValueFieldName" : "ddmFieldValueKeyword_en_US"
   }
],
```

Depending on your version, [nested field storage for DDM fields](https://learn.liferay.com/web/guest/w/dxp/liferay-internals/reference/7-3-breaking-changes#dynamic-data-mapping-fields-in-elasticsearch-have-changed-to-a-nested-document) may be enabled by default for Elasticsearch:

| Liferay Version  | Nested Field Enabled by Default |
| ---------------- | ------------------------------- |
| 7.4 all updates  | ✔                               |
| 7.3 all updates  | ✔                               |
| DXP 7.2 SP3/FP8+ | ✘                               |

To change the behavior, use the _Enable Legacy Dynamic Data Mapping Index Fields_ setting in System Settings → Dynamic Data Mapping Indexer.

See [Boosting Matches to Nested Fields](https://learn.liferay.com/web/guest/w/dxp/using-search/search-pages-and-widgets/search-results/custom-filter-examples#boosting-matches-to-nested-fields) for an example showing the use of a web content structure field with the Custom Filter widget.

\
