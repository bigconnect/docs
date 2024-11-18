# REST query import

The REST query import feature allows you to import one or more queries at a time.

The **supported formats** for import are:

* Swagger 2.0 / OpenAPI 2.0 (YAML or JSON)
* Swagger 3.0 / OpenAPI 3.0 (YAML or JSON)
* CURL

&#x20;

### Import collection

#### File

Upload a file containing your API documentation.

<figure><img src="https://files.readme.io/6a2a11c-file.png" alt=""><figcaption></figcaption></figure>

&#x20;

#### Text

Enter the documentation string directly.

<figure><img src="https://files.readme.io/f40c6c1-raw.png" alt=""><figcaption></figcaption></figure>

&#x20; &#x20;

***

&#x20;

### Converting and importing a Postman collection

As Postman does not natively use OpenAPI, you will need to generate an OpenAPI definition before importing it into Mware Application Development Framework.

Find the collection in Postman that you want to export. Click the `...` and select `Export`.

<figure><img src="https://files.readme.io/eacbb98-export_collection.png" alt="Exporting an API collection in Postman"><figcaption><p>Exporting an API collection in Postman</p></figcaption></figure>

This will download a JSON file with the API configuration.

We now need to convert this collection to an OpenAPI definition. We can do that with the handy [postman2openapi](https://github.com/kevinswiber/postman2openapi) tool.

<figure><img src="https://files.readme.io/1b157c6-openapi.png" alt=""><figcaption></figcaption></figure>

In Mware Application Development Framework, add a new REST API datasource, or select an existing one. Under the _Queries_ tab, click `Import`.

<figure><img src="https://files.readme.io/fa6b77e-Screenshot_2023-11-23_at_09.05.17.png" alt=""><figcaption></figcaption></figure>

Click on the _Raw Text_ tab and paste in the newly generated OpenAPI definition. Click `Import`.

<figure><img src="https://files.readme.io/7691a92-raw_text.png" alt=""><figcaption></figcaption></figure>

You will now see the imported requests listed:

<figure><img src="https://files.readme.io/2592a69-Screenshot_2023-11-23_at_09.57.51.png" alt="Imported API collection"><figcaption><p>Imported API collection</p></figcaption></figure>
