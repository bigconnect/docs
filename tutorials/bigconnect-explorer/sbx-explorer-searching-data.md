# Searching Data

BDL is powered by ElasticSearch as a search engine, so it supports most of the features ElasticSearch does with regards to searching and aggregation.

In this tutorial we will explore how to use some of the search features of BigConnect Explorer. 

Login to BigConnect Explorer using the default username **admin** and password **admin.** Click on the **Discover** menu item in the top menu bar and you will be taken to the search page.

![](../../.gitbook/assets/image%20%28465%29.png)

In the top part you have a global search field where you can enter search terms. Have a look at the **Querying data** wizard on the search page to get some hints on what is the search syntax.

To quickly summarize, the following search operators are supported in the search field:

* **Wildcards**: _qu?ck bro\*_
* **Fuzzy**: _quikc~ brwn~ foks~_
* **Proximity** **searches**: _"fox quick"~5_
* **Term Boosting**: _quick^2 fox_
* **Boolean operators**: _AND, OR, NOT_
* **Term Grouping**: _\(quick OR brown\) AND fox_

On the left side of the search page you have the **Filters**. Here you can choose what you want to search for \(Entities or Relationships, the properties to filter on and sort conditions\). These work  on new searches and for filtering existing searches.

Let's do a search for everything by typing **\*** in the search field:

![](../../.gitbook/assets/image%20%28366%29.png)

We have a new panel on the right side called **Refinements**. By default, BigConnect Explorer aggregates all search results by some predefined system properties: Concept and Media Tag in our case. You can drill down in search results by clicking on a concept or media tag.

For example, if we want to see all objects that have the Media Tag = animal, click on the animal media tag and the search results will be filtered accordingly.

![](../../.gitbook/assets/image%20%28211%29.png)

You can further drill down on available Refinements or use Filters to do so.

{% hint style="info" %}
The Refinements panel is configurable. You can choose with properties to aggregate on from the Administration -&gt; Schema -&gt; Properties section. Choose a desired property and check the Search Facet field, and set an appropriate Aggregation for that property.

Note: the property must be indexed \(eg. Text Index Hints &lt;&gt; None\)
{% endhint %}





