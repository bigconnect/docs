# Discover

This is where you search through data stored in BigConnect. 

![](../../../.gitbook/assets/image%20%28474%29.png)

In the top part you have a global search field where you can enter search terms. Have a look at the **Querying data** wizard on the search page to get some hints on what is the search syntax.

To quickly summarize, the following search operators are supported in the search field:

* **Wildcards**: _qu?ck bro\*_
* **Fuzzy**: _quikc~ brwn~ foks~_
* **Proximity** **searches**: _"fox quick"~5_
* **Term Boosting**: _quick^2 fox_
* **Boolean operators**: _AND, OR, NOT_
* **Term Grouping**: _\(quick OR brown\) AND fox_

On the left side of the search page you have the **Filters**. Here you can choose what you want to search for \(Entities or Relationships, the properties to filter on and sort conditions\). These work  on new searches and for filtering existing searches.

To see what's available in the system, type \* in the search field and hit Enter:

![](../../../.gitbook/assets/image%20%28372%29.png)

We have a new panel on the right side called **Refinements**. By default, BigConnect aggregates all search results by some predefined system properties: Concept and Media Tag in our case. You can drill down in search results by clicking on a concept or media tag.

For example, if we want to see all objects that have the Media Tag = animal, click on the animal media tag and the search results will be filtered accordingly.

![](../../../.gitbook/assets/image%20%28214%29.png)

You can further drill down on available Refinements or use Filters to do so.

## Filtering

Search results can be filtered by:

* **Concepts**:  add one or more concepts 
* **Relation**: add one or more types of relations
* **Property**: add one or more properties 

Filtering by Properties depends on the type of the property being selected. Different operators are available based on the type of property, as follows:

* **String**: 
  * _CONTAINS WORD_: the Property value must contain the specified word
  * _EQUALS_: the Property value must exactly have the specified value
  * _HAS `Property`_: the Property is set
  * _DOES NOT HAVE `Property`_: the Property is not set
* **Numeric**: 
  * _LESS THAN, GREATHER THAN_: the Property value must be strictly bigger or smaller than the specified value \(equality is not considered\)
  * _LESS THAN EQUAL, GREATHER THAN EQUAL_: the Property value must be bigger or smaller than the specified value \(equality is considered\)
  * EQUALS: the Property value must equal the specified value
  * _HAS `Property`_: the Property is set
  * _DOES NOT HAVE `Property`_: the Property is not set
* **Date**: 
  * _BEFORE, AFTER_: the Property value must be strictly before or after the specified date \(equality is not considered\)
  * _BEFORE EQUAL, AFTER EQUAL_: the Property value must be before or after the specified value \(equality is considered\)
  * _BETWEEN_: the Property value must be between the provided dates
  * _EQUALS_: the Property value must equal the specified value
  * _HAS `Property`_: the Property is set
  * _DOES NOT HAVE `Property`_: the Property is not set
* **Geolocation**: 
  * LATITUDE, LONGITUDE: The location latitude and longitude must equal the provided values
  * KM: radius in km of a circle with center \(LATITUDE, LONGITUDE\) to search for objects

Multiple property filters can be applied. By default, the operator applied between subsequent filters is AND. However, you can change this to OR or NOT.

You can also group property filters together using the grouping icons: ![](../../../.gitbook/assets/image%20%28598%29.png) 

## Sorting

Search results can be sorted by one or more properties. Select the desired properties from the dropdown and click on them afterwards to change the sort order.

## Saved searches

You can save the current query, its filters, refinements and sort properties by clicking the Saved Searches dropdown:

![](../../../.gitbook/assets/image%20%28700%29.png)

Just enter a name for your saved search and choose whether you want it to be available to all users \(Global\). By default, a saved search is available only to the user that created it.

{% hint style="info" %}
Saved Searches are used in the Saved Search dashboard widget and in tools like Graph, Map and Charts to load multiple items from the graph store.
{% endhint %}

## Exporting search results

You can export all search results or only selected ones. The top export icon next to the Saved Searches dropdown ![](../../../.gitbook/assets/image%20%28650%29.png) is used to export all search results. 

![](../../../.gitbook/assets/image%20%28698%29.png)

If you want to export only certain search results, select the rows you want to export and click the export dropdown next to the pagination options.  

![](../../../.gitbook/assets/image%20%28701%29.png)

