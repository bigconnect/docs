# Elasticsearch

Using Elasticsearch as a data source in Mware Application Development Framework&#x20;

Elasticsearch is a distributed, RESTful search and analytics engine capable of addressing a growing number of use cases.

### Connect

To start, click on the `Add source` button of the **Data** section, select _Elasticsearch_, and click `Continue`.

Next enter the connection configuration to your Elasticsearch service. The URL field should include the protocol, address and port as shown below.

Choose `Save and continue to query`.

<figure><img src="https://files.readme.io/2d1deb7-image.png" alt=""><figcaption></figcaption></figure>

### Adding a query

After configuring the connection to your Elasticsearch Datasource, choose the `Add query` button to perform a CRUD operation against your instance.

<figure><img src="https://files.readme.io/2bf4198-image.png" alt=""><figcaption></figcaption></figure>

#### Read

This is the default _Function_. Specify an index to read from, and provide a JSON query payload. An example query is shown below but you can use the Inspect function in Kibana to generate more advanced queries.

<figure><img src="https://files.readme.io/31db696-read_query.png" alt=""><figcaption></figcaption></figure>

Query

```json
{
  "query": {
    "bool": {
      "must": [
        {
          "bool": {
            "should": [
              {
                "match_phrase": {
                  "text": "Javan"
                }
              }
            ],
            "minimum_should_match": 1
          }
        }
      ],
      "filter": [],
      "should": [],
      "must_not": []
    }
  }
}
```

Scroll down and use the _Run Query_ button to preview the results of your query.

![](https://files.readme.io/43dd7cc-image.png)

After successfully returning results use the `Save Query` button to ensure your query can be used by your app in the Design section.

You can also add and use a Binding within your query as shown below:

<figure><img src="https://files.readme.io/d0a3d1c-Screenshot_2023-01-31_at_15.43.46.png" alt=""><figcaption></figcaption></figure>

> ### 📘
>
> More on using datasource bindings within the Design section can be found in the Custom queries page.

#### Create

Select _Create_ from the _Function_ dropdown. Provide the name of the Elasticsearch index you want to use and provide a JSON document payload in the _query_ field.

<figure><img src="https://files.readme.io/bf50785-Screenshot_2023-01-31_at_15.55.19.png" alt=""><figcaption></figcaption></figure>

Unlike the read query, you can immediately `Save Query` without running first, however it may be worth clicking `Run Query` to verify that the query is correct, but be sure to save after.

When running the query, some information will be returned, such as an **\_id**, which can be used when _updating_ and _deleting_.

Create Response

```json
{
  "_index": "bible",
  "_id": "YLOLCIYB1PeB0MyAV4tF",
  "_version": 1,
  "result": "created",
  "_shards": "[object Object]",
  "_seq_no": 31104,
  "_primary_term": 5
}
```

#### Update

Updating involves providing an **index** and **\_id**. Select the _Update_ function from the dropdown, and enter the fields.

To update the fields of an existing document, your query may look like:

<figure><img src="https://files.readme.io/79343a0-Screenshot_2023-01-31_at_17.00.04.png" alt=""><figcaption></figcaption></figure>

There are a number of additional update operations you can take, which are documented in the [Official docs](https://www.elastic.co/guide/en/elasticsearch/reference/current/docs-update.html) page.

#### Delete

Select the _Delete_ function and provide an **index** and document **\_id** to be deleted. No query is required. You may also `Save Query` without running beforehand.

<figure><img src="https://files.readme.io/3f6e027-Screenshot_2023-01-31_at_17.23.14.png" alt=""><figcaption></figcaption></figure>
