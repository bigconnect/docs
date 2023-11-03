# Secure APIs using OAuth2 Access Tokens

APIs published on WSO2 API Gateway can be secured by OAuth 2.0, which is the de facto standard for access delegation in the REST API world. Any client application invoking an OAuth2 secured API needs to have a valid subscription to that particular API and present a valid OAuth2.0 Access Token when invoking it. For more information on how to subscribe to an application to an API and generate credentials for it, see [Subscribe to an API](https://apim.docs.wso2.com/en/4.2.0/consume/manage-subscription/subscribe-to-an-api/).

After you have got the required credentials, namely the consumer key and consumer secret for your application, you (application users) can obtain access tokens to invoke the APIs that are subscribed under the given application. WSO2 API Manager offers a set of OAuth2 grant types for obtaining access tokens depending on the type of the access token owner, type of the application, and the trust relationship with the application. For more information, see [OAuth2 Grant Types](https://apim.docs.wso2.com/en/4.2.0/design/api-security/oauth2/grant-types/overview/).

### OAuth 2.0 access token types <a href="#oauth-20-access-token-types" id="oauth-20-access-token-types"></a>

WSO2 APIM only allows creating applications of JWT token type. See [JWT Access Tokens](https://apim.docs.wso2.com/en/4.2.0/design/api-security/oauth2/access-token-types/jwt-tokens). Hence all the access tokens issued under any application are JWTs.

### Authentication via access tokens <a href="#authentication-via-access-tokens" id="authentication-via-access-tokens"></a>

HTTP Authorization header is the most common method of providing authentication information for REST APIs. The access token (followed by **Bearer**) needs to be sent via the authorization header, for the client application to authenticate the API that is being accessed. The format of the header is as follows:

Format

```bash
Default Format
    Authorization : Bearer <access-token>

Format for customized authorization header
    <customized-authorization-header> : Bearer <access-token>
```

Example

For more information, see [Customizing Authorization Header](https://apim.docs.wso2.com/en/latest/design/api-security/api-authentication/secure-apis-using-oauth2-tokens/#customizing-the-authorization-header).

### Customizing the authorization header <a href="#customizing-the-authorization-header" id="customizing-the-authorization-header"></a>

If the value of the authorization header needs to be changed due to organizational policies, legacy APIs, or for any other reason, it can be done at three levels in WSO2 API Manager (WSO2 API-M) as described below.

* [Customize across all Tenants](https://apim.docs.wso2.com/en/latest/design/api-security/api-authentication/secure-apis-using-oauth2-tokens/#customize-across-all-tenants)
* [Customize per Tenant](https://apim.docs.wso2.com/en/latest/design/api-security/api-authentication/secure-apis-using-oauth2-tokens/#customize-per-tenant)
* [Customize per API](https://apim.docs.wso2.com/en/latest/design/api-security/api-authentication/secure-apis-using-oauth2-tokens/#customize-per-api)

#### Customize across all tenants <a href="#customize-across-all-tenants" id="customize-across-all-tenants"></a>

Follow the instructions below to change the value of the authorization header at the server level configurations:

1. Navigate to the `<API-M_HOME>/repository/conf/deployment.toml` file and make the following changes.
   1.  Uncomment the `auth_header` under the `[apim.oauth_config]` section and add the customized authorization header.

       You need to make this change on all the profiles of WSO2 API Manager.

       Format

       ```bash
        auth_header = "{authorization-header}"
       ```

       Example
   2.  Add the custom authorization header to the list of allowed headers defined in the `allow_headers` under the `[apim.cors]` section.

       Format

       ```bash
        allow_headers = {authorization-header}
       ```

       Example
2. Restart the WSO2 API Manager server to reload the changes.
3. If you have already published and deployed APIs, sign in to the WSO2 API Publisher (`https://<hostname>:9443/publisher`) and save and redeploy those APIs.

#### Customize per tenant <a href="#customize-per-tenant" id="customize-per-tenant"></a>

Each tenant can define their name for the authorization header by following the instructions below:

Note

The tenant wide authorization header customizations take precedence over the server-wide authorization header customizations.

1.  Sign in to the [WSO2 API Manager Management Console](https://apim.docs.wso2.com/en/4.2.0/install-and-setup/install/installing-the-product/running-the-api-m/#accessing-the-management-console) with your tenant credentials.

    `https://<server-host>:9443/carbon`

    Tip

    For information on creating a tenant, see [Managing Tenants](https://apim.docs.wso2.com/en/4.2.0/administer/multitenancy/managing-tenants/).
2.  Click **Main**, then click **Browse** which is under **Resources**.

    ![Registry Browse](https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/registry-browse.png)
3. Enter / `_system/config/apimgt/applicationdata/tenant-conf.json` as the location and click **Go** to access the `tenant-conf.json` file that is in the WSO2 Registry.
4. Click **Edit as text** to be able to edit the JSON file.
5.  Add the following configuration with the customized authorization header to the JSON file and save the file.

    Format

    ```bash
    "AuthorizationHeader" : "<tenant-authorization-header>"
    ```

    ExampleSample JSON
6. If you have already published and deployed the APIs, [sign in to the API Publisher](https://apim.docs.wso2.com/en/4.2.0/install-and-setup/install/installing-the-product/running-the-api-m/#accessing-the-api-publisher) using your tenant credentials, and save and redeploy those APIs.

#### Customize per API <a href="#customize-per-api" id="customize-per-api"></a>

The API Publisher application allows an API Developer or Product Manager to specify the name of the authorization header when creating or modifying an API.

Note

The customized authorization header defined per API takes precedence over the customized authorization headers that are defined server and tenant wide.

Follow the instructions below to add a customized authorization header for an API:

1. [Sign in to the Publisher](https://apim.docs.wso2.com/en/4.2.0/install-and-setup/install/installing-the-product/running-the-api-m/#accessing-the-api-publisher). `https://<server-host>:9443/publisher`
2.  When [creating](https://apim.docs.wso2.com/en/4.2.0/design/create-api/create-rest-api/create-a-rest-api/) or updating an API, define the customized authorization header in the **Runtime** tab under **Develop -> API Configurations**.

    ![Custom Auth Header](https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/custom-auth-header-publisher.png)
3. Save and Deploy the API.

Note

When creating an API by importing a Swagger or OpenAPI definition, the user can define the customized authorization header in the OAS definition using the **“x-wso2-auth-header”** extension.

```yaml
x-wso2-auth-header: "CustomAuthorizationHeader"
```

### Try out the customized authorization header <a href="#try-out-the-customized-authorization-header" id="try-out-the-customized-authorization-header"></a>

Before you start, sign in to the API Publisher and deploy the sample API (`PizzaShackAPI`) if you have not done so already, as the following example is based on the `PizzaShackAPI` API.

Sample cURL Request

```bash
curl -H "<customized-authorization-header>: Bearer <access-token>" -H "accept: application/json" "https://<server-IP>:<port>/pizzashack/1.0.0/menu" -k -v
```

cURL Request FormatSample Response

Note that the customized authorization header appears in the Developer Portal UI as well.

![Customized Auth Header](https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/auth\_header.png)
