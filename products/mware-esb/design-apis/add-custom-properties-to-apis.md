# Add Custom Properties to APIs

Usually, APIs have a predefined set of properties such as the name, version, context, etc. However, there may be instances where you want to add specific custom properties to your API. You can do this in either of the following ways:

* [Add custom properties via the API Publisher](https://apim.docs.wso2.com/en/latest/design/create-api/adding-custom-properties-to-apis/#AddcustompropertiesviatheAPIPublisher)
* [Add custom properties via the REST API](https://apim.docs.wso2.com/en/latest/design/create-api/adding-custom-properties-to-apis/#AddcustompropertiesviatheRESTAPI)

When adding custom properties, note the following:

* Property name should be unique.
* Property name should not contain spaces.
* Property name cannot be case-sensitive.
* Property name cannot be any of the following as they are reserved keywords: provider, version, context, status, description, subcontext, doc, lcState, name, tags.

After the custom properties have been added, you can [search for APIs using custom property values](https://apim.docs.wso2.com/en/latest/design/create-api/adding-custom-properties-to-apis/#Searchusingcustomproperties).

## Add custom properties via the API Publisher <a href="#add-custom-properties-via-the-api-publisher" id="add-custom-properties-via-the-api-publisher"></a>

1.  Sign in to the API Publisher as an API creator using the following URL:

    `https://<localhost>:9443/publisher`
2. [Create a new API](https://apim.docs.wso2.com/en/4.2.0/design/create-api/create-rest-api/create-a-rest-api) or edit an existing API.
3.  Click **Properties** and click **ADD NEW PROPERTY**.



    <figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/properties-add-property.png" alt=""><figcaption></figcaption></figure>
4.  Enter a custom property name and value (e.g., property name: environment, property value: preprod), mark Developer Portal visibility as appropriate and click **ADD** to add it.



    <figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/add-new-property.png" alt=""><figcaption></figcaption></figure>
5. Click **SAVE** to save the API.

## Add custom properties via the REST API <a href="#add-custom-properties-via-the-rest-api" id="add-custom-properties-via-the-rest-api"></a>

Use the [existing REST API](https://apim.docs.wso2.com/en/4.2.0/develop/product-apis/restful-apis/) to add a new API and in order to add the API with custom properties make sure to add the following element to the request body including the relevant properties.

`"additionalProperties : {"environment": "preprod", "secured": "true"}`

## Search using custom properties <a href="#search-using-custom-properties" id="search-using-custom-properties"></a>

You can use the following format to search for an API using the custom properties:

`<property_name>:<property_value>`

For example, if you want to search for the **environment** property with a specific value (e.g., preprod) in the Publisher, you can search as shown below:

<figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/search-apis-with-custom-properties.png" alt=""><figcaption></figcaption></figure>

When you click on the name of the API in the above screen, the respective API Overview page appears. Click on the **Properties** tab to list the API properties that you added.

<figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/view-custom-api-properties.png" alt=""><figcaption></figcaption></figure>
