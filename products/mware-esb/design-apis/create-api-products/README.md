# Create API Products

An API Product is a packaging mechanism that you can use when you need to bundle a preferred set of resources from multiple APIs and expose it as a separate API interface, which can be consumed by subscribers. API Products give Publishers the ability to repackage their existing APIs in various combinations to provide a tailor-made experience for their subscribers.

WSO2 API Manager allows Publishers to create API Products via the Publisher Portal. Subscribers will see the API Product via the Developer Portal as a separate entity, which is independent of the APIs with which it shares its resources. From a subscriber's perspective, the API Product will look and function in the same way as any of the standard APIs on the Developer Portal.

## How it works <a href="#how-it-works" id="how-it-works"></a>

Let's use the following example to understand how API Products work in WSO2 API Manager.

**Example**

<figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/design-api/create-api-product/api-product-overview.png" alt=""><figcaption></figcaption></figure>

A financial institution has the following three APIs.

*   **Customer Info API**

    This API is used to access and update the financial information of the customers. The API consists of the following resources.

    | Resources                          | Description                                                         |
    | ---------------------------------- | ------------------------------------------------------------------- |
    | **GET /customers**                 | Get the list of all the existing customers.                         |
    | **GET /customers/{customerId}**    | Get details of a specific customer based on the customer ID.        |
    | **POST /customers**                | Add a new customer.                                                 |
    | **PUT /customers/{customerId}**    | Update details of a specific customer based on his/her customer ID. |
    | **DELETE /customers/{customerId}** | Delete a specific customer based on his/her customer ID.            |
*   **Leasing API**

    Used to access and update the information regarding the leases of the institution. The API consists of the following resources.

    | Resources                    | Description                                             |
    | ---------------------------- | ------------------------------------------------------- |
    | **GET /assets**              | Get the list of all existing assets that can be leased. |
    | **GET /assets/{assetId}**    | Get details of a specific asset based on an asset ID.   |
    | **POST /assets**             | Add a new asset.                                        |
    | **PUT /assets/{assetId}**    | Update a specific asset based on an asset ID.           |
    | **DELETE /assets/{assetId}** | Delete a specific asset based on an asset ID.           |
*   **Credit API**

    Used to access and update the credit information with regard to a specific customer. The API consists of the following resources.

    | Resources                        | Description                                                                  |
    | -------------------------------- | ---------------------------------------------------------------------------- |
    | **GET /accounts/{customerId}**   | Get the list of credit accounts for a customer based on his/her customer ID. |
    | **GET /accounts/{accountId}**    | Get details of a specific credit account based on an account ID.             |
    | **POST /accounts/{customerId}**  | Add a new credit account for a customer based on a customer ID.              |
    | **PUT /accounts/{accountId}**    | Update a specific credit account based on an account ID.                     |
    | **DELETE /accounts/{accountId}** | Delete a specific credit account based on an account ID.                     |

The resources of the above mentioned existing APIs can be reused and exposed via API Products as shown below.

*   **Leasing API Product**

    | Resources                       | Description                                                  | API               |
    | ------------------------------- | ------------------------------------------------------------ | ----------------- |
    | **GET /customers**              | Get the list of all the existing customers.                  | Customer Info API |
    | **GET /customers/{customerId}** | Get details of a specific customer based on the customer ID. |                   |
    | **GET /assets**                 | Get the list of all the existing assets that can be leased.  | Leasing API       |
    | **GET /assets/{assetId}**       | Get details of a specific asset based on the asset ID.       |                   |
*   **Credit API Product**

    | Resources                       | Description                                                       | API               |
    | ------------------------------- | ----------------------------------------------------------------- | ----------------- |
    | **GET /customers**              | Get the list of all the existing customers.                       | Customer Info API |
    | **GET /customers/{customerId}** | Get details of a specific customer based on the customer ID.      |                   |
    | **GET /accounts/{accountId}**   | Get details of a specific credit account based on the account ID. | Credit API        |
    | **POST /accounts/{customerId}** | Add a new credit account for a customer based on a customer ID.   |                   |

In this example, a subset of the resources from different APIs have been combined to provide a customized hybrid offering that can appeal to different subscribers. This also ensures that only specific resources are exposed via the API Product.
