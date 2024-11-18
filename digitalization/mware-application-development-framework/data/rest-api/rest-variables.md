# REST variables

Variables are a useful way to reuse data across REST queries. Variables come in two forms; static and dynamic.

<figure><img src="https://files.readme.io/aa44cd1-Screenshot_2022-01-04_at_17.08.13.png" alt=""><figcaption></figcaption></figure>

***

### Static Variables

Static variables store data that does not change. These can range from constants used in the metadata of the request to information about the request itself that can be used in the URL such as API version or hostname.

Variables nested inside other variables will not be evaluated.

<figure><img src="https://files.readme.io/6334065-Screenshot_2022-01-04_at_17.10.35.png" alt=""><figcaption></figcaption></figure>

***

### Dynamic Variables

Dynamic variables provide a way to chain data between queries.

* Response data from one query can be used to create a dynamic variable
  * Header
  * Body field
* The result of the variable will be cached for a short period of time
* A variable will be re-evaluated if a query using it fails, in which case the query will retry once
* A query will fail if a variable cannot be evaluated

&#x20;

#### Header Variables

Use the **...** menu alongside response headers to select **Create dynamic variable**

<figure><img src="https://files.readme.io/eaeaecd-Screenshot_2022-01-04_at_17.27.24.png" alt=""><figcaption></figcaption></figure>

Give the variable a name.

* The name must be unique
* The name will be used to bind this variable in your query. e.g _cookie_ can be used as _\{{cookie\}}_

<figure><img src="https://files.readme.io/3051a94-Screenshot_2022-01-04_at_17.28.22.png" alt=""><figcaption></figcaption></figure>

The variable will appear in the **Dynamic Variables** tab

<figure><img src="https://files.readme.io/301bb4f-Screenshot_2022-01-04_at_17.32.24.png" alt=""><figcaption></figcaption></figure>

&#x20;

#### Body Variables

Use the ... menu alongside schema items to select **Create dynamic variable**

<figure><img src="https://files.readme.io/21d418f-Screenshot_2022-01-04_at_17.33.45.png" alt=""><figcaption></figcaption></figure>

Give the variable a name as above

<figure><img src="https://files.readme.io/cbec0f3-Screenshot_2022-01-04_at_17.34.50.png" alt="" width="375"><figcaption></figcaption></figure>

The variable will appear in the **Dynamic Variables** tab

<figure><img src="https://files.readme.io/b53eeae-Screenshot_2022-01-04_at_17.35.29.png" alt=""><figcaption></figcaption></figure>

&#x20;

#### Editing Dynamic Variables

In the above example we can see the _user\_id_ variable is bound to _\{{ data.0.\[user] \}}_ which is an object. To access nested data in the response body we can update the variable expression to reference the nested fields using dot notation e.g. _\{{ data.0.\[user.\_id] \}}_ .\
Alternatively, a query transformer can be used to update the schema so that the desired field is exposed in the schema directly.

Dynamic variables can also be created manually using the Add variable button.

* Access header data using

```
{{ info.headers.[header-name] }}
```

* Access body data using

```
{{ data.0.[body-field] }}
```

<figure><img src="https://files.readme.io/a946bc3-Screenshot_2022-01-04_at_17.40.52.png" alt=""><figcaption></figcaption></figure>

&#x20;

Once a variable has been defined it can be used in the exact same way as a binding.

* For example, a variable named **my-id** can be referenced in the headers, params, or body of a query using _\{{my-id\}}_
* For more details see REST Bindings
