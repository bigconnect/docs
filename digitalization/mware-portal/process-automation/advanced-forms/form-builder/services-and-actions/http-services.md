# HTTP services

The HTTP Service Editor allows you to create simple REST services. The idea is that a form can call a service, typically passing XML back and forth.

To create a new HTTP service, click the Add icon under "HTTP Services". The HTTP Service Editor opens.

The following screenshot shows an example of filled-out service:

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-6c4c1469423b962a67b2d9176a219e4ac506cbc2%252Fservice-definition.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=478cf8049e866eb20be832d1f9fd3119f78013a1c49313204d331c1f8f8a7671)

Definition tab

### Service definition <a href="#service-definition" id="service-definition"></a>

#### Basic settings <a href="#basic-settings" id="basic-settings"></a>

The "Definition" tab allows you to set the basic service parameters:

* **Service Name**
  * This is the name of the service, as seen by Form Builder. Must start with a letter, and may not contain spaces.
  * \[SINCE Orbeon Forms 2020.1] The service can be renamed, and actions that use the service are automatically updated.
* **Resource URL**
  * `HTTP` or `HTTPS` URL to which the service must be called.
  *   The value is an XPath Value Template, which means that the URL can be dynamic. For example, with Orbeon Forms 2018.1's `fr:control-string-value()` function:

      Copy

      ```
      https://example.org/{fr:control-string-value('control-1')}
      ```
* **Method**
  * The HTTP method to use: `GET`, `POST`, `PUT` or `DELETE`.
* **Request Body**
  * This applies to the `POST` and `PUT` methods.
  * The XML document to send to the service. When using the "HTML Form" serialization, this is converted to name/value pairs.

#### Serializations <a href="#serializations" id="serializations"></a>

**Basics**

Serializations apply to the `POST` and `PUT` methods only.

**XML serialization**

With the "XML" serialization, the XML data is passed using the [external data format](https://doc.orbeon.com/form-runner/api/data-formats/form-data).

On the wire, this serialization uses the `application/xml` content type.

**HTML Form serialization**

With the "HTML Form" serialization, leaf XML elements in the XML request body are converted to name/value pairs.

On the wire, this serialization uses the `application/x-www-form-urlencoded` encoding, like for standard web forms.

#### URL parameters <a href="#url-parameters" id="url-parameters"></a>

\[SINCE Orbeon Forms 2016.1]

* This applies to the `GET` and `DELETE` methods.
* You can add as many URL parameters as needed.
* A non-blank URL parameter specifies a default value for the parameter.
* An action can set the value of a parameter.

Here is [how to set URL parameters from an action](https://doc.orbeon.com/form-builder/advanced/services-and-actions/actions#passing-url-parameters-to-get-and-delete-methods).

#### JSON support <a href="#json-support" id="json-support"></a>

\[SINCE Orbeon Forms 2016.1]

A JSON _response_ from a service is processed and converted to an XML document following the XForms 2.0 scheme (see [JSON support](https://doc.orbeon.com/xforms/submission/submission-json)).

See [#2480](https://github.com/orbeon/orbeon-forms/issues/2480) for details about sending JSON to services.

#### URL Parameters before Orbeon Forms 2016.1 <a href="#url-parameters-before-orbeon-forms-2016.1" id="url-parameters-before-orbeon-forms-2016.1"></a>

Prior to Orbeon Forms 2016.1, a "request body" is mandatory for the `GET` and `DELETE` methods. The body is not sent to the service, but instead is used to configure request parameters.

The content of the "Request Body" form has to be a well-formed XML document. The name of the root element doesn't matter, but usually `params`or `request` is used. Each child element defines a parameter as shown in the following example:

Copy

```
<params>
    <userId>1</userId>
    <userName>test</userName>
</params>
```

Here Orbeon invokes the URL:

Copy

```
$<Resource URL>?userId=1&userName=test
```

where `$<Resoure URL>` is the content of the input field "Resource URL".

Make sure to select `HTML Form` in the `Serialization` dropdown, otherwise the URL parameters are not appended to the request URL.

### Advanced parameters <a href="#advanced-parameters" id="advanced-parameters"></a>

The "Advanced" tab allows you to set advanced service parameters:

* **Service Response has Binary Content**
  * \[SINCE Orbeon Forms 2020.1]
  * Check this if you know that the service returns binary content, such as an image. This is necessary if the [`<fr:control-setattachment>`](https://doc.orbeon.com/form-builder/advanced/services-and-actions/actions-syntax#setting-the-value-of-an-attachment-control) action makes use of the response.
* **HTTP Authentication**
  * Whether to use HTTP authentication.
  * **Username:** Username to use.
  * **Password:** Password to use.
* **SOAP Service**
  * Whether this is a SOAP service
* **SOAP Action**
  * If selected, the value of the `SOAPAction` header.

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-e3fcd92f5084e89a7bd3b644e3cdf62a2d5e4099%252Fservice-definition-advanced.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=9709df51479b53a4dbec43373c7a9b0850ed01dc41293a3561a57fe3bd768d08)

Advanced tab

### Testing a service <a href="#testing-a-service" id="testing-a-service"></a>

#### Basics <a href="#basics-1" id="basics-1"></a>

The "Test" button allows you to test the service. Before doing this, you have to set data in the request body for a `POST` or `PUT` request, or you might want to set URL parameters for a `GET` or `DELETE`. Form Builder executes the service, and then provides information about the response returned, including:

* Whether an error occurred (green or red highlight)
* URL called
* Response status code
* Response headers
* Response body

This helps you troubleshoot the service call.

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-19d9031ea8616daafd206b1aca220bdf37630bc4%252Fservice-definition-test.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=4b1446f415f79978f3cefccafe3d6f97091143fe55459fedeed7d2fc276eb68c)

Test Results tab with response headers

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-d9617881848a23389f292ed043e64092f82f88eb%252Fservice-definition-test-original-response-body.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=5f45296884c3f263c877126540914e8bd5e4a5109c6aa50bb30f15431e9e5e7d)

Test Results tab with response body

#### XML view of JSON responses <a href="#xml-view-of-json-responses" id="xml-view-of-json-responses"></a>

\[SINCE Orbeon Forms 2019.1]

When the service response is in JSON format, you can view the JSON response _translated into XML_ following [XForms 2.0](https://www.w3.org/community/xformsusers/wiki/XForms\_2.0#External\_JSON\_values).

This is useful when you want to use XPath expressions to access a JSON response, for example with [datasets](https://doc.orbeon.com/form-runner/features/datasets) or [item choices](https://doc.orbeon.com/form-builder/advanced/services-and-actions/actions#setting-the-choices-of-a-selection-control).

See also [JSON support](https://doc.orbeon.com/xforms/submission/submission-json).

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-655b2e8f87a673f5b7f92887603a9a33c782189a%252Fservice-definition-test-xml-response-body.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=5f802c33cb5f7dd2674a989149523bd230308dfce4bd6bd4d89554443951b7c9)

Test Results tab with XML response body

### Saving the service <a href="#saving-the-service" id="saving-the-service"></a>

Once your service is defined, the "Save" buttons saves it to the form. You can come back to it and modify it later by clicking on the "Edit" icon next to the service name. You can also delete the service using the trashcan icon.

### Deleting a service <a href="#deleting-a-service" id="deleting-a-service"></a>

You can delete a saved service using the "Remove" button.

\[SINCE Orbeon Forms 2020.1]

Orbeon Forms warns you if there are existing actions that refer to the service.
