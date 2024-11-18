# REST bindings

Use bindings to supply additional information to a query at runtime. In this example we have:

* An API endpoint that accepts a status field to filter a list of applications
* A query named _applications_
* An application that supplies data to the bindings of the query

<figure><img src="https://files.readme.io/79188f5-Screenshot_2022-01-04_at_15.19.06.png" alt=""><figcaption></figcaption></figure>

&#x20;

***

&#x20;

### Creating and using bindings

We can create a binding named _status_ with a default value of _all_.

<figure><img src="https://files.readme.io/85475c5-Screenshot_2022-01-04_at_13.31.34_1.png" alt=""><figcaption></figcaption></figure>

&#x20;

Using handlebars the binding can be referenced throughout the query in the following places:

* URL
* Params
* Headers
* Body
* Transformer

&#x20;

#### Params example

The binding can be used as the value of a param of the same name. While the name can be shared it is not required.

<figure><img src="https://files.readme.io/df1c2e0-Screenshot_2022-01-04_at_13.44.11.png" alt=""><figcaption></figcaption></figure>

&#x20;

#### Headers example

The binding can be used as the value of a header of the same name. As above the binding and header name may or may not be shared.

<figure><img src="https://files.readme.io/faddba2-Screenshot_2022-01-04_at_14.52.40.png" alt=""><figcaption></figcaption></figure>

#### Body example

The binding can be used in any part of the request body. e.g. as the value of a JSON field.

<figure><img src="https://files.readme.io/2ee1845-Screenshot_2022-01-04_at_15.43.15.png" alt=""><figcaption></figcaption></figure>

&#x20;

### Using REST queries in forms

You may wish to populate Default Values with the properties of your GET query, or allow users to submit form data via a POST query.

Learn more about Bindings in forms.

&#x20;
