---
layout:
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# GraphQL APIs

GraphQL, which has been developed by Facebook, is a data query language for APIs. When using GraphQL, users can explicitly specify as to what data they need from an API. GraphQL APIs are an alternative to REST-based APIs.

You can use a Schema Definition Language (SDL) schema to design a GraphQL API in WSO2 API Manager (WSO2 API-M) similar to creating SOAP APIs using WSDLs and developing REST APIs using OpenAPI Specifications (a.k.a. Swagger Definitions).

All GraphQL schemas generally have three operation root types, which are namely query, mutation, and subscription. Therefore, every request against a GraphQL endpoint of a GraphQL server should have a payload starting with any one of the root types, including its related operation name. You can manage the security, authorization, and rate limiting aspect of each operation based on its operation name.

{% hint style="info" %}
GraphQL client supports only `graphql-transport-ws` protocol but the Gateway supports both the `graphql-transport-ws` and `graphql-ws` protocols.
{% endhint %}

Follow the instructions below to design a GraphQL API.

## Step 1 - Start the GraphQL backend server

Let's use the Star Wars sample backend server as the backend for the GraphQL API.

1. Clone the [WSO2 API Manager Samples](https://github.com/wso2/samples-apim) repository.

```
git clone https://github.com/wso2/samples-apim
```

2. Navigate to `graphql-backend` directory.

```
cd samples-apim/graphql-backend
```

3. Run `npm install` to install the necessary node modules
4. Run `npm start` to start the server

Once the above steps are done, the Star Wars server will be running on `http://localhost:8080`.

<figure><img src="../../../../.gitbook/assets/image (879).png" alt=""><figcaption></figcaption></figure>

You can use `http://localhost:8080/graphql` as the endpoint when creating the GraphQL API.

## Step 2 - Design a GraphQL API

1.  Sign in to the API Publisher Portal.

    `https://<hostname>:9443/publisher`

    Example: `https://localhost:9443/publisher`

    Let's use `admin` as the username and password to sign in.
2. Click **Create API** and then click **Import GraphQL SDL**.

<figure><img src="../../../../.gitbook/assets/create-graphql-schema-option.webp" alt=""><figcaption></figcaption></figure>

3. Import the schema by dragging and dropping the file or by uploading the file, and click **Next**.

Let's use the [StarWarsAPI schema definition](https://apim.docs.wso2.com/en/latest/assets/attachments/learn/schema\_graphql.graphql) to create the schema file.

{% hint style="info" %}
* You need to define the SDL Schema based on the [GraphQL schema design best practices](https://leapgraph.com/graphql-schema-design-best-practices).
* The file extension can be either `.graphql`, `.txt`, or `.json`.
* The file name can be any name, which is based on your preference.
{% endhint %}

<figure><img src="../../../../.gitbook/assets/image (880).png" alt=""><figcaption></figcaption></figure>

4. Enter the GraphQL API related details and click **Create**.

Let's create an API named "StarWarsAPI" using the following sample data.

<table data-header-hidden><thead><tr><th width="136"></th><th></th></tr></thead><tbody><tr><td>Field</td><td>Description</td></tr><tr><td>Name</td><td>StarWarsAPI</td></tr><tr><td>Context</td><td><code>/swapi</code></td></tr><tr><td>Version</td><td>1.0.0</td></tr><tr><td>Endpoint</td><td><p>http://localhost:8080/graphql</p><ul><li>When you provide the HTTP URL as the backend endpoint, WSO2 API-M will internally derive the corresponding WebSocket URL <code>ws://localhost:8080/graphql</code>.</li><li>Thereafter, the API Gateway will use this WebSocket URL as the backend subscription endpoint of the GraphQL API.</li></ul></td></tr></tbody></table>

<figure><img src="../../../../.gitbook/assets/image (881).png" alt=""><figcaption></figcaption></figure>

5.  Optionally, modify the existing GraphQL schema definition.

    a. Navigate to **Develop**, **API Configurations**, and click **Schema Definition**.

    b. Click **Download Definition**.

    The existing GraphQL API schema gets downloaded.\


    <figure><img src="../../../../.gitbook/assets/image (882).png" alt=""><figcaption></figcaption></figure>

    c. Update the schema definition as required.

    d. Click **Import Definition** to import the updated schema definition.



6. Update the GraphQL API operations as required.

Instead of resources, which get populated for REST APIs, operations get populated for GraphQL APIs.

a. Click **Show More** under the **Operations** section in the **Overview** page to navigate to the operations page.

<div data-full-width="true">

<figure><img src="../../../../.gitbook/assets/image (883).png" alt=""><figcaption></figcaption></figure>

</div>

b. Update the operations as required.

The Publisher can add Rate Limiting policies, scopes and enable/disable security for each of the GraphQL API operations.

**i. Create scopes**.

Repeat the following sub-steps to create two scopes named `adminScope` and `FilmSubscriberScope`.

**i.a.** Click **Local Scopes**, and then click **Create Scopes**.

<figure><img src="../../../../.gitbook/assets/image (884).png" alt=""><figcaption></figcaption></figure>

**i.b.** Enter the required details.

Enter the following details for this example scenario.

<table data-header-hidden><thead><tr><th width="302"></th><th></th></tr></thead><tbody><tr><td><strong>Name</strong></td><td><strong>Role</strong></td></tr><tr><td><code>FilmSubscriber</code></td><td><code>FilmSubscriber</code></td></tr><tr><td><code>adminScope</code></td><td><code>admin</code></td></tr></tbody></table>



<figure><img src="../../../../.gitbook/assets/image (885).png" alt=""><figcaption></figcaption></figure>

**i.c.** Press `Enter` to add each role.

**i.d.** Click **Save**.

<figure><img src="../../../../.gitbook/assets/image (886).png" alt=""><figcaption></figcaption></figure>

**ii.** **Define the operation level configurations**.

**ii.a.** Click **Operations**.

**ii.b.** Click **Operation Level** to apply Rate Limiting for operations.

<figure><img src="../../../../.gitbook/assets/image (887).png" alt=""><figcaption></figcaption></figure>

**ii.c.** Select a Rate Limiting Policy, scope, and enable or disable security for each of the operations.

Apply the following scopes to the respective operations.

| **Operation**   | **Scope**        |
| --------------- | ---------------- |
| `allDroids`     | `FilmSubscriber` |
| `allCharacters` | `adminScope`     |

**ii.d.** Click **Save**.

If you check the list of scopes, it should appear as follows:

<figure><img src="../../../../.gitbook/assets/image (888).png" alt=""><figcaption></figcaption></figure>

Now, you have successfully created and configured a GraphQL API.

Next, let's deploy your API in the API Gateway and Publish your API.

## GraphQL operations <a href="#graphql-operations" id="graphql-operations"></a>

The following sub-sections explain as to how authorization, security, and throttling affect API operations.

### **Authorization for GraphQL operations**

A scope acts as a limiting factor on what API resources can be accessed using an access token and thereby defines the authorization aspect for API requests.

*   **Authorization for a single operation**

    When a query has a single operation, the access token should include the scope that is attached to that specific operation to be able to invoke the API.
*   **Authorization for multiple operations**

    When a query has multiple operations, the access token should include all the scopes that are attached to the operations that correspond to that specific API to be able to invoke the API.

### **Security for GraphQL operations**

Security can be enabled or disabled for GraphQL operations. Security is enabled for GraphQL operations by default.

*   **Security for a single operation**

    When a query has an operation that has security enabled, the users need to enter their credentials to invoke the respective API. However, when security is disabled for the operation, the users can invoke the respective API without entering their credentials.
*   **Security for multiple operations**

    The API request takes into consideration the security configurations of all the operations that belong to the API. When a query has security enabled for one of the operations that belong to a specific API, then security is automatically applied for all the operations. Therefore, in such cases, users have to use their credentials when invoking that respective API.

### **Rate limiting for GraphQL operations**

Rate limiting is set to unlimited, and thereby disabled by default.

*   **Rate limiting for a single operation**

    When a query has an operation with a rate limiting policy applied to it, the respective rate limiting policy is taken into account to rate limit the request when it exceeds the desired limit.
*   **Rate limiting for multiple operations**

    WSO2 API Manager checks all the operation related rate limiting policies when determining the overall rate limit. When the requests exceed the minimum rate limit, which corresponds to the operations in the query, they will be throttled out.
