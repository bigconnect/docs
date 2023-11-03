# Add API Documentation

**API documentation** helps API subscribers to understand the functionality of the API and API publishers to market their APIs better and sustain competition. Using the API Publisher, you can add different types of documentation from various sources. All documents created in the API Publisher have unique URLs to help improve SEO support.

The documentation types supported in the API Publisher are as follows:

* **In-line**: Hosts documentation (How-tos, Samples, SDK, forums etc.) in WSO2 API Publisher itself and allows it to be edited directly via the UI.
* **URL**: Links of public or support forums or file references (URLs) of an external configuration management system.
* **File**: Allows to upload the documentation directly to the server.
* **Markdown**: Allows to add markdown (`.md`) docs via a Markdown editor.

Follow the instructions below to add documentation to an API:

1.  Sign in to the API Publisher.

    `https://<hostname>:9443/publisher`
2.  Click on the API (e.g., `PizzaShackAPI 1.0.0` ) for which you want to add the documentation.

    [![Select API in API Publisher](https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/select-api.png)](https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/select-api.png)
3.  Add the specific documentation.

    Follow the steps below for each type.

    * [Add in-line documentation](https://apim.docs.wso2.com/en/latest/design/api-documentation/add-api-documentation/#add-in-line-documentation)
    * [Add documentation using a URL](https://apim.docs.wso2.com/en/latest/design/api-documentation/add-api-documentation/#add-documentation-using-a-url)
    * [Add documentation using a file](https://apim.docs.wso2.com/en/latest/design/api-documentation/add-api-documentation/#add-documentation-using-a-file)
    * [Add Markdown documentation](https://apim.docs.wso2.com/en/latest/design/api-documentation/add-api-documentation/#add-markdown-documentation)

    #### &#x20;<a href="#add-in-line-documentation" id="add-in-line-documentation"></a>

## Add in-line documentation <a href="#add-in-line-documentation" id="add-in-line-documentation"></a>

1.  Click **Documents** and click **Add New Document**.



    <figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/add-docs-documents.png" alt=""><figcaption></figcaption></figure>
2.  Enter the following details to create an in-line document.

    |         |                                                           |
    | ------- | --------------------------------------------------------- |
    | Name    | PizzaShack                                                |
    | Summary | This is the official documentation for the PizzaShack API |
    | Type    | How To                                                    |
    | Source  | In-line                                                   |



    <figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/add-docs-create-inline.png" alt=""><figcaption></figcaption></figure>
3. Click **Add Document**.
4.  Click **Add Content** to open an embedded editor.



    Note

    You can edit the content later by clicking **Edit Content**. It will open up the embedded editor to proceed.



    <figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/add-docs-add-content.png" alt=""><figcaption></figcaption></figure>

    <figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/add-docs-edit-content.png" alt=""><figcaption></figcaption></figure>
5.  Edit the document content in-line using the embedded editor and click **Update Content**.



    The API's **Documents** tab opens.\


    <figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/add-docs-update-content.png" alt=""><figcaption></figcaption></figure>

## Add documentation using a URL <a href="#add-documentation-using-a-url" id="add-documentation-using-a-url"></a>

You can use either **Public Forum**, **Support Forum** or **Other** types to add documentation using a URL.

### **Add a document using a URL based on a Public Forum/Support Forum**

1. Click **Add New Document** to add another documentation type.
2.  Enter the following information to create another doc using a URL of a Public or Support forum.

    | Name    | APIForum                                                   |
    | ------- | ---------------------------------------------------------- |
    | Summary | Forum to discuss about APIs                                |
    | Type    | Public Forum/Support Forum                                 |
    | Source  | <p>URL<br>&#x3C;<code>http://www.dummyforum.com</code></p> |



    <figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/add-docs-forum-type.png" alt=""><figcaption></figcaption></figure>

### **Add an other type URL based document**

You can use this if you want to add a document using the **Other** type that points to a link that has a file reference of an external source.

1.  Enter the following information to create another doc using a URL.

    | Name                | PizzaShackWiki                                                                                                  |
    | ------------------- | --------------------------------------------------------------------------------------------------------------- |
    | Summary             | Sample PizzaShack API                                                                                           |
    | Type                | Other                                                                                                           |
    | Other Document Type | Other (Provide a type as required)                                                                              |
    | Source              | <p>URL<br><a href="http://www.dummy.com/PizzaShackAPIDoc.pdf">http://www.dummy.com/PizzaShackAPIDoc.pdf</a></p> |



    <figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/add-docs-url-type.png" alt=""><figcaption></figcaption></figure>
2.  Click **Add Document**.

    The API's **Documents** tab opens.

## Add documentation using a file <a href="#add-documentation-using-a-file" id="add-documentation-using-a-file"></a>

1. Click **Add New Document** to add yet another document using a file.
2.  Enter the following information.

    |         |                                                                                                                                                                                                               |
    | ------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Name    | APIManagerSamples                                                                                                                                                                                             |
    | Summary | API Manager Samples                                                                                                                                                                                           |
    | Type    | Samples & SDK                                                                                                                                                                                                 |
    | Source  | You can provide only these file formats.(`.pdf`, `.doc`, `.txt` ) of any size. For example, use the sample PDF file [here](https://apim.docs.wso2.com/en/4.2.0/assets/attachments/learn/api-docs-sample.pdf). |



    <figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/add-docs-pdf-file.png" alt=""><figcaption></figcaption></figure>
3. Click **Add Document**.

## Add Markdown documentation <a href="#add-markdown-documentation" id="add-markdown-documentation"></a>

1.  Click **Documents** and click **Add New Document**.



    <figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/add-docs-documents.png" alt=""><figcaption></figcaption></figure>
2.  Enter the following details to create Markdown documentation and click **Add Document**.

    |         |                                                           |
    | ------- | --------------------------------------------------------- |
    | Name    | PizzaShackAPIDocumentation                                |
    | Summary | This is the official documentation for the PizzaShack API |
    | Type    | How To                                                    |
    | Source  | Markdown                                                  |



    <figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/add-docs-markdown.png" alt=""><figcaption></figcaption></figure>
3.  Click **Add Content** to open an embedded editor.

    ![Add content for Markdown documentation](https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/add-docs-markdown-add-content.png)
4.  Edit the document content using the embedded markdown editor and click **Update Content**.



    The API's **Documents** tab opens.



    <figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/add-docs-markdown-editor.png" alt=""><figcaption></figcaption></figure>

    <figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/add-docs-markdown-view.png" alt=""><figcaption></figcaption></figure>

You have now added four type of documents to the API: in-line, URL, file, and markdown.



<figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/add-docs-all.png" alt=""><figcaption></figcaption></figure>

1.  Sign in to the WSO2 Developer Portal and click on the `PizzaShackAPI` version 1.0.0 API.

    `https://<hostname>:9443/devportal`



    <figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/view-docs-devportal.png" alt=""><figcaption></figcaption></figure>
2.  Click **Documents**.

    The documents that you added, which are listed by type, appears.
3.  Use the dropdown and select the document that you want to view.

    As a subscriber, you can read the documentation and learn about the API.

    !!! note For REST APIs, generated document will be listed as `Default`



    <figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/view-docs-api.png" alt=""><figcaption></figcaption></figure>

You have created documentation using the API Publisher and viewed the documentation as a subscriber in the Developer Portal.

## Control API Document Visibility in Developer Portal <a href="#control-api-document-visibility-in-developer-portal" id="control-api-document-visibility-in-developer-portal"></a>

By default, any document associated with an API has the same visibility level of the API. That is, if the API is public, its documentation is also visible to all users (registered and anonymous).

### **Enabling document visibility in Developer Portal**

To enable other visibility levels to the documentation, go to the `<API-M_HOME>/repository/conf/deployment.toml`, add the following configuration.

```toml
[apim.publisher]
enable_api_doc_visibility = "true"
```

Document visibility levels can be one of the following options: - **Same as API visibility:** visible to the same user roles who can see the API. For example, if the API's visibility is public, its documentation is visible to all users. - **Visible to My Domain:** visible to all registered users in the API's tenant domain. - **Publishers Only:** visible only to the users who have permission to log in to the API Publisher web interface and create and/or publish APIs to the Developer Portal.

### **Control API document visibility using the API Publisher**

1. Sign in to the API Publisher as an API creator using the following URL: `https://<localhost>:9443/publisher`
2. [Create a new API](https://apim.docs.wso2.com/en/4.2.0/design/create-api/create-rest-api/create-a-rest-api/) or edit an existing API.
3.  In the **Documents** tab, Click **Add New Document**, to see a new drop-down list added to select visibility from.&#x20;

    <figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/api-doc-visibility-selector.png" alt=""><figcaption></figcaption></figure>
4.  Select the desired visibility level for the document.&#x20;

    <figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/api-doc-visibility-dropdown.png" alt=""><figcaption></figcaption></figure>
5. Add the Document.
