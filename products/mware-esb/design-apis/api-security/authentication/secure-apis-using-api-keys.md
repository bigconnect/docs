# Secure APIs using API Keys

An API key is the simplest form of application-based security that you can configure for an API. You can obtain an API key for a client application from WSO2 API Manager's Developer Portal, via the UI, or via REST APIs. Thereafter, the client application can use the API key to invoke the APIs that are secured with the API key security scheme.

WSO2 API Manager uses a self-contained JSON Web Token (JWT) as the API key, and this JWT access token is generated via the Developer Portal without communicating with the Key Manager.

When an API is invoked specifying an API key as the authentication method, the APIM Gateway performs the following two basic validations.

* Signature validation
* Subscription validation

## Prerequisites for API keys <a href="#prerequisites-for-api-keys" id="prerequisites-for-api-keys"></a>

* The API key should be a valid JWT signed using the primary KeyStore private key of the Developer Portal.
*   The expected token format is as follows:

    `base64(header).base64(payload).base64(signature)`
*   The public certificate of the private key that is used to sign the tokens should be added to the trust store under the `"gateway_certificate_alias"` alias. For more information, see [Import the public certificate into the client trust store.](https://apim.docs.wso2.com/en/latest/design/api-security/api-authentication/secure-apis-using-api-keys/#import)

    Note

    The latter mentioned prerequisite is not applicable if you use the default certificates, which are the certificates that are shipped with the product itself.

## Validation of API subscriptions <a href="#validation-of-api-subscriptions" id="validation-of-api-subscriptions"></a>

The subscription validation is mandatory for the API keys, and the keys generated before an application subscribes to an API will not contain the subscription information under the token details. As a result, these keys will not be allowed to access that specific API. Therefore, API Keys should be generated after the application has subscribed to the required API.

In order to enable API subscription validation, do the following:

1. Navigate to the `<PRODUCT-HOME>/repository/conf/deployment.toml` file.
2.  Add the following configuration in the ‘deployment.toml’ file

    ```toml
    [apim.key_manager]
    enable_apikey_subscription_validation = true
    ```
3. Restart the API-M server.

## Using API keys to secure an API <a href="#using-api-keys-to-secure-an-api" id="using-api-keys-to-secure-an-api"></a>

Follow the instructions below to use API key Authentication in WSO2 API Manager.

### Step 1 - Create and publish an API <a href="#step-1-create-and-publish-an-api" id="step-1-create-and-publish-an-api"></a>

Create, publish and deploy an API that is secured with the API key security scheme as the application-level security. Let's work with the sample app for this purpose.

1.  Sign in to the Publisher.

    `https://<hostname>:9443/publisher`
2. Click **DEPLOY SAMPLE API** to deploy the sample PizzaShack API.
3. Click **Develop -> API Configurations -> Runtime** and select **Application Level Security**.
4.  Select **API Key** and click **Save and Deploy**.



    <figure><img src="https://apim.docs.wso2.com/en/4.1.0/assets/img/learn/api-key-option.png" alt=""><figcaption></figcaption></figure>

### Step 2 - Generate the API Key <a href="#step-2-generate-the-api-key" id="step-2-generate-the-api-key"></a>

1.  Sign in to the Developer Portal.

    `https://<hostname>:9443/devportal`
2. Click **APIs** and click on the respective API (e.g., `PizzaShackAPI`).
3. Click **Subscriptions**.
4.  Select an application and select a throttling policy.

    Note

    API Keys can work with any application, which is either JWT or OAuth.
5.  Click **Subscribe**.



    <figure><img src="https://apim.docs.wso2.com/en/4.1.0/assets/img/learn/subscribe-to-api.png" alt=""><figcaption></figcaption></figure>
6.  Click **MANAGE APP**, corresponding to the application that you used to subscribe to the API.



    <figure><img src="https://apim.docs.wso2.com/en/4.1.0/assets/img/learn/view-credentials-manage-app.png" alt=""><figcaption></figcaption></figure>
7.  Click **API KEY** and click **GENERATE KEY**.



    <figure><img src="https://apim.docs.wso2.com/en/4.1.0/assets/img/learn/generate-api-key.png" alt=""><figcaption></figcaption></figure>
8.  Optionally, define a validity period for the token.

    By default, the API Key does not expire. However, optionally, you can define a validity period for the token as follows:

    1. When you click **Generate Keys,** deselect the **API Key with infinite validity period** option in the pop-up.
    2. Enter the expiry time in seconds.
9.  Copy the API key.



    <figure><img src="https://apim.docs.wso2.com/en/4.1.0/assets/img/learn/copy-api-key.png" alt=""><figcaption></figcaption></figure>

### Step 3 - Invoke the API <a href="#step-3-invoke-the-api" id="step-3-invoke-the-api"></a>

Invoke the API using the API key. You can use either of the following methods to invoke the API.

*   Specify the API Key in the `apikey` header.

    Format

    ```bash
    curl -k -X GET "https://localhost:8243/pizzashack/1.0.0/menu" -H "accept: application/json" -H "apikey: <API_key_value>"
    ```

    ExampleResponse
*   Specify as a query parameter in the API request.

    * `<url_encoded_API_key_value>` - Encode the API key using a URL encoder (e.g., [https://www.urlencoder.org](https://www.urlencoder.org/)).

    Format

    ```bash
    curl -k -X GET "https://localhost:8243/pizzashack/1.0.0/menu?apikey=<url_encoded_API_key_value>"
    ```

    ExampleResponse

## Additional Information <a href="#additional-information" id="additional-information"></a>

### Importing the public certificate into the client trust store <a href="#importing-the-public-certificate-into-the-client-trust-store" id="importing-the-public-certificate-into-the-client-trust-store"></a>

Note

Make sure to import the Developer Portal certificate to the APIM Gateway client-truststore under the same alias.

Follow the instructions below to import the public certificate into the client trust store.

1. Navigate to the `<API-M_HOME>/repository/resources/security/` directory.
2.  Run the following command to export the public certificate from WSO2 API Manager's key store (`wso2carbon.jks`).&#x20;

    `keytool -export -alias wso2carbon -file wso2.crt -keystore wso2carbon.jks`
3. Enter `wso2carbon` as the default password of the key store when prompted.
4.  Run the following command to import the public certificate into the trust store.&#x20;

    ```
    keytool -import -trustcacerts -keystore client-truststore.jks -alias gateway_certificate_alias -file wso2.crt
    ```
5. Enter `wso2carbon` as the default password of the trust store when prompted.

### Changing the alias name in the JWT <a href="#changing-the-alias-name-in-the-jwt" id="changing-the-alias-name-in-the-jwt"></a>

By default, the alias name is `gateway_certificate_alias`. Follow the instructions below if you need to change the alias name in the JWT.

1. Navigate to the `<API-M_HOME>/repository/conf/deployment.toml` file.
2.  Configure the `api_key_alias` value under `[apim.devportal]` as follows:

    ```
    [apim.devportal]
    api_key_alias = "<alias-name>"
    ```

### API key restriction for IP address and HTTP referrer <a href="#api-key-restriction-for-ip-address-and-http-referrer" id="api-key-restriction-for-ip-address-and-http-referrer"></a>

After issuing an API key for an application, it can be used by anyone to invoke an API subscribed to the application. However, if an unauthorized party gets hold of the token, they can create unnecessary invocations to the APIs. To prevent this issue, you can define the authorized parties when generating a token.

WSO2 API Manager allows API keys to be restricted based on two approaches.

#### **1) IP address restriction**

The IP address restriction allows only the clients with specific IP addresses to use the token. The IP addresses can be specified in the following formats.

* IPv4 (e.g., `192.168.1.2`)
* IPv6 (e.g., `2002:eb8::2`)
* IP range in CIDR notation (e.g. `152.12.0.0/13`, `1001:ab8::/14`)

**Generating an API key with an IP restriction**

1. Navigate to API key generation window of the specific application in the Developer Portal.
2. Select `IP Addresses`, add the IP addresses in the text input as shown below, and generate the key.

<figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/ip-api-key.png" alt=""><figcaption></figcaption></figure>

#### **2) HTTP referrer restriction**

When the HTTP referrer restriction has been enabled, only the specific HTTP referrers can use the token. Therefore, by using this restriction, when API clients run on web browsers, you can limit the access to an API through only specific web pages. The referrer can be specified in the following formats.

* A specific URL with an exact path: `www.example.com/path`
* Any URL in a single subdomain, using a wildcard asterisk (\*): `sub.example.com/*`
* Any subdomain or path URLs in a single domain, using wildcard asterisks (\*): `*.example.com/*`

**Generating an API key with the HTTP referrer restriction**

1. Navigate to API key generation window of that specific application in the Developer Portal.
2. Select `HTTP Referrers (Web Sites)` and add the referrers in the text input as shown below and generate the key.

<figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/http-referer-api-key.png" alt=""><figcaption></figcaption></figure>
