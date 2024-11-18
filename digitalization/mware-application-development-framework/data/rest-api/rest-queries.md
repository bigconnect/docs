# REST queries

### Creating a new request

First, create a REST datasource if you do not have one already. For more details, see: REST. Click the **Add query** button to open the query editor.

<figure><img src="https://files.readme.io/90a4546-Screenshot_2022-01-04_at_11.22.12_1.png" alt=""><figcaption></figcaption></figure>

***

#### Name your query

Give your query a recognizable name that will appear in the datasource navigator. For example, a query that retrieves a list of applications could be called applications.

<figure><img src="https://files.readme.io/79f159c-Screenshot_2022-01-04_at_11.35.09.png" alt=""><figcaption></figcaption></figure>

***

#### API endpoint URL

Enter the URL of the API endpoint you are using. The URL is the requested resource including the protocol, domain name, and path.

<figure><img src="https://files.readme.io/d3b6ce9-Screenshot_2022-01-04_at_13.20.07.png" alt=""><figcaption></figcaption></figure>

> ### 📘
>
> If you are trying to connect to a service on the same host, please look at this page: [docs.docker.com/desktop/networking](https://docs.docker.com/desktop/networking/#i-want-to-connect-from-a-container-to-a-service-on-the-host)

***

#### Method

Select the HTTP method used by the API endpoint you are using.

* GET retrieves data from an API.
* POST sends new data to an API.
* PATCH and PUT update existing data.
* DELETE removes existing data.

![](https://files.readme.io/a124afd-Screenshot\_2022-01-04\_at\_11.37.37.png)

***

#### Access level

Select the access level required by the logged-in end-user. If the user visits a page where a query is executed and the access level is beyond the scope of the user's access the query will not run.

![](https://files.readme.io/4b0b571-Screenshot\_2022-01-04\_at\_13.21.49.png)

***

#### Params

URL parameters can be configured in the params tab.

* Parameters are appended to the end of the request URL, following _?_ and listed in key value pairs, separated by _&_ using the following syntax: _?status=all\&other=value_
* Parameters can be hardcoded or they can use bindings or variables.

<figure><img src="https://files.readme.io/fc4c06e-Screenshot_2022-01-04_at_13.34.14.png" alt=""><figcaption></figcaption></figure>

***

#### Bindings

Use bindings to supply additional information to the query at runtime. Using the example from above we can replace the hardcoded value of _all_ with a binding named _status_ that has a default value of _all_. Now we can change the value of _status_ by supplying it from within an application using the query.

<figure><img src="https://files.readme.io/3600cb6-Screenshot_2022-01-04_at_13.31.34.png" alt=""><figcaption></figcaption></figure>

***

#### Headers

Use headers to supply key-value pairs accepted by your API.

* Switch the Active toggle to off to prevent the headers from being sent while keeping it's configuration.
* Headers can be hardcoded or they can use REST Bindings or REST Variables.

<figure><img src="https://files.readme.io/0569efd-Screenshot_2022-01-04_at_15.38.48.png" alt=""><figcaption></figcaption></figure>

***

#### Body

Use a request body to send information to an API endpoint.

* The Content-Type header will automatically be set when using:
  * raw(JSON) / application/json
  * raw(XML) / application/xml
  * raw(Text) / text/plain
* If you manually select a Content-Type header, that value will be overridden by the body type
* The body can be hardcoded or it can use [REST Bindings](https://docs.budibase.com/docs/rest-bindings) or [REST Variables](https://docs.budibase.com/docs/rest-variables) .

<figure><img src="https://files.readme.io/6762fb3-Screenshot_2022-01-04_at_15.44.42.png" alt=""><figcaption></figcaption></figure>

***

#### Transformer

Transformers can be used to augment the format of the returned data.

<figure><img src="https://files.readme.io/8dc9b53-Screenshot_2022-01-04_at_16.07.50.png" alt=""><figcaption></figcaption></figure>

***

#### Authentication

Preset authentication strategies configured at the REST data source level can be used to authenticate queries. For more information on using REST Authentication.

![](https://files.readme.io/6b960c3-Screenshot\_2022-01-04\_at\_16.11.02.png)

***

#### Sending the request

When your query is crafted you can use the **Send** button to preview the results of your query. When your query has been successfully executed you may save the query using **Save Query**

<figure><img src="https://files.readme.io/920a65b-Screenshot_2022-01-04_at_15.59.24.png" alt=""><figcaption></figcaption></figure>

#### Posting form data to a REST query

If you have a [Form](https://docs.budibase.com/docs/forms)  setup in a screen, you can pass bindings through to a POST query to handle a submission.

This is done in a similar manner as posting form data to a custom query.

***

### Using response data

When your query is crafted you can use the **Send** button to preview the results of your query. When your query has been successfully executed you may save the query using **Save Query**.

The saved query can then be used as the data source of a Data Provider or by the Execute Query action.

***

#### Modifying the schema

The response schema may be updated by:

* Adding or removing fields
* Changing the data type used when displaying the result in tables

<figure><img src="https://files.readme.io/496ce40-Screenshot_2022-01-04_at_16.03.14.png" alt=""><figcaption></figcaption></figure>

***

#### Creating dynamic variables

The response schema and headers can be used to create dynamic variables. For more information on static and dynamic variables see REST Variables

<figure><img src="https://files.readme.io/3aaa81c-Screenshot_2022-01-04_at_16.04.27.png" alt=""><figcaption></figcaption></figure>

<figure><img src="https://files.readme.io/a83f2cc-Screenshot_2022-01-04_at_16.05.44.png" alt=""><figcaption></figcaption></figure>

***

#### Displaying nested data

If you REST query returns an array of objects, Mware Application Development Framework will automatically generate a nested schema. This allows you to display related nested data in a table or any other format.

As an example, this GET request will return a list of sections that have been changed on a Wikipedia page: `<https://api.wikimedia.org/core/v1/wikipedia/en/revision/847170467/compare/851733941>`

Output

```json
{
    "from": {
        "id": 847170467,
        "slot_role": "main",
        "sections": [
            {
                "level": 2,
                "heading": "==Description==",
                "offset": 3006
            },
            {
                "level": 2,
                "heading": "==Taxonomy==",
                "offset": 4324
            },
            {
                "level": 3,
                "heading": "===Family placement===",
                "offset": 4643
            },
          ...
```

To get the data into a format suitable for a Table, we will first Transform this response to surface the properties at the top-level:

JavaScript

```javascript
const from = data.from
const to = data.to

return {
	fromId: from.id,
	fromSlotRole: from.slot_role,
	fromSections: from.sections,
	toId: to.id,
	toSlotRole: to.slot_role,
	toSections: to.sections
}
```

Click `Send` and `Save`. Looking at the `Schema` we can see that the 'from' and 'to' sections appear as type **Array**. Behind the scenes Mware Application Development Framework understands that the items are objects and will generate appropriate types based on the data.

<figure><img src="https://files.readme.io/4757704-Screenshot_2024-02-21_at_16.17.08.png" alt=""><figcaption></figcaption></figure>

With our query created, and schema generated, navigate to the **Design** section. Add a Repeater [Block](https://docs.budibase.com/docs/blocks) and select the query as the data source.

<figure><img src="https://files.readme.io/e19dcd9-Screenshot_2024-02-21_at_16.23.50.png" alt="Repeater block with REST query data source"><figcaption><p>Repeater block with REST query data source</p></figcaption></figure>

Next add a [Table block](https://docs.budibase.com/docs/table-block) and nest it under the repeater block. Under the _JSON Arrays_ header, select the 'fromSections' as the data source.

<figure><img src="https://files.readme.io/df5c127-Screenshot_2024-02-21_at_17.11.41.png" alt=""><figcaption></figcaption></figure>

Click the lightning bolt under the _Title_ and select the 'fromId' binding: `FROM: {{ Revision Repeater Block.api_wikimedia_org revision.fromId }}`

Finally [duplicate](https://docs.budibase.com/docs/components#editing) the table block, and rename 'from' into 'to', selecting 'toSections' as the data source.

<figure><img src="https://files.readme.io/25559c2-Screenshot_2024-02-21_at_17.15.43.png" alt=""><figcaption></figcaption></figure>
