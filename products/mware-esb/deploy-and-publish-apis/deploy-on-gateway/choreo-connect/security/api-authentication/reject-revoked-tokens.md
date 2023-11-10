# Reject Revoked Tokens

After issuing an access token or an API Key, a user or admin can revoke it due to any reason. This must be done especially if it has been compromised. If a revoked token is used to invoke an API, Choreo Connect will reject the invocation by returning an Unauthenticated Error response.

Important

Revoking the access token can only be done by enabling the Control Plane Event hub. This is when running [Choreo Connect with WSO2 API Manager as a Control Plane](https://apim.docs.wso2.com/en/4.2.0/deploy-and-publish/deploy-on-gateway/choreo-connect/concepts/apim-as-control-plane/). For standalone mode, the only workaround would be to undeploy the API and redeploy with a different name or the version, while keeping the context (basepath) unchanged.

Info

When you revoke an access token using APIM, an event will be sent to Choreo Connect via the Control Plane Event hub and Choreo Connect will reject the token then onwards. You can follow the [Quick Start Guide](https://apim.docs.wso2.com/en/4.2.0/deploy-and-publish/deploy-on-gateway/choreo-connect/getting-started/quick-start-guide-docker-with-apim/) to have an initial setup of Choreo Connect with Control Plane enabled.

Follow the steps given below to revoke an access token or an API key

* [Revoking an Access Token](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/choreo-connect/security/rejecting-revoked-tokens/#revoking-an-access-token)
* [Revoking an API Key](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/choreo-connect/security/rejecting-revoked-tokens/#revoking-an-api-key)

### Revoking an Access Token[¶](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/choreo-connect/security/rejecting-revoked-tokens/#revoking-an-access-token) <a href="#revoking-an-access-token" id="revoking-an-access-token"></a>

Here are the steps to revoke an Access Token.

1. [Deploy the API](https://apim.docs.wso2.com/en/4.2.0/deploy-and-publish/deploy-on-gateway/choreo-connect/deploy-api/deploy-rest-api-in-choreo-connect).
2. Generate an access token from APIM (Follow [Get a Test Key to Invoke an API](https://apim.docs.wso2.com/en/4.2.0/consume/invoke-apis/invoke-apis-using-tools/invoke-an-api-using-the-integrated-api-console/#get-a-test-key-to-invoke-an-api)).
3.  Revoke the access token by calling the following API from control plane.

    Format

    ```
    curl -k -v -d "token=<ACCESS_TOKEN_TO_BE_REVOKED>" -H "Authorization: Basic <base64 encoded (clientId:clientSecret)>" -H "Content-Type: application/x-www-form-urlencoded" https://localhost:9443/oauth2/revoke
    ```

    ExampleResponse

    Info

    Once the token is revoked, if you check the logs in `adapter`, you will see the following logs appear, which means the `adapter` has received the token revocation data from the control plane.

    ```
    2021-05-01 07:20:57 INFO [revoked_token_listener.go:39] - [messaging.handleTokenRevocation] [-] Event JWT is received []
    2021-05-01 07:20:57 INFO [server.go:1315] - [xds.UpdateEnforcerRevokedTokens] [-] New Revoked token cache update for the label: commonEnforcerLabel version: 652374093
    ```
4.  Verify by invoking the API using revoked access token. See the following example.

    Example

    ```
        curl -X '<your method>' 'https://localhost:9095/<your api context>/<your api version>/<your resource>' -H 'accept: application/json'   -H 'Content-Type: application/json' -H 'Authorization: Bearer eyJ4NXQiOiJNell4TW1Ga09HWXdNV0kwWldObU5EY3hOR1l3WW1NNFpUQTNNV0kyTkRBelpHUXpOR00wWkdSbE5qSmtPREZrWkRSaU9URmtNV0ZoTXpVMlpHVmxOZyIsImtpZCI6Ik16WXhNbUZrT0dZd01XSTBaV05tTkRjeE5HWXdZbU00WlRBM01XSTJOREF6WkdRek5HTTBaR1JsTmpKa09ERmtaRFJpT1RGa01XRmhNelUyWkdWbE5nX1JTMjU2IiwiYWxnIjoiUlMyNTYifQ.eyJzdWIiOiJ2YWppcmEtZGV2IiwiYXV0IjoiQVBQTElDQVRJT05fVVNFUiIsImF1ZCI6IkhEZVBINFluRHdjOWtsUHRROTY3MnpGVWZYa2EiLCJuYmYiOjE2MTk4NTIyOTYsImF6cCI6IkhEZVBINFluRHdjOWtsUHRROTY3MnpGVWZYa2EiLCJzY29wZSI6InJlYWQ6cGV0cyIsImlzcyI6Imh0dHBzOlwvXC9sb2NhbGhvc3Q6OTQ0M1wvb2F1dGgyXC90b2tlbiIsImV4cCI6MTYxOTg1NTg5NiwiaWF0IjoxNjE5ODUyMjk2LCJqdGkiOiI0ZTMzMGRiZC03NWM2LTRhYjUtYTc0OS1iOTQ5N2FhMzUyMWEifQ.jUzkrljwJu77FRGswRQ1uKBRDsHswwp1oRyjh-ULOf4pZ0faxCTKO2djpBBkxVtgaiv1YKtzhaqRUV_UgCLDAgVO76G-sntSXmN5ATEohwcJfSNRHzOsfiCem-kkgO6P2TE15x7G4hlTfufIiirVlXVjcv-4s3fTVCCd-rsm7JEgb9HM-W2GV6l8-CDJ1cfzEQxSnQ7hgueO9CzhTbSnfRNZrwxZtNxXQC8LZcKfNQAzBbOlScT0qB4iJj_FgOHdIlpLANC-IFeCO6UnChztGRqccyTWUSjNuQBtN3_jCb1MQig3m7b2ZSirDygnZ09oecxxxUC2Sj1mHM5i6LCsEA'   -d '<your-content>' -v -k
    ```

    Response

### Revoking an API Key[¶](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/choreo-connect/security/rejecting-revoked-tokens/#revoking-an-api-key) <a href="#revoking-an-api-key" id="revoking-an-api-key"></a>

Here are the steps to revoke an API Key.

1. [Deploy the API](https://apim.docs.wso2.com/en/4.2.0/deploy-and-publish/deploy-on-gateway/choreo-connect/deploy-api/deploy-rest-api-in-choreo-connect).
2. Generate an API Key from API-M by following the steps in [Secure APIs with API Keys](https://apim.docs.wso2.com/en/4.2.0/design/api-security/api-authentication/secure-apis-using-api-keys).
3. Get an access token to the Control Plane (API-M) by referring to [this section](https://apim.docs.wso2.com/en/4.2.0/reference/product-apis/devportal-apis/devportal-v3/devportal-v3/#section/Authentication). Remember to include `apim:api_key` as a scope when requesting the access token.
4.  Revoke the API Key by calling the following API from the Control Plane.

    Format

    ```
    curl -k -v -H "Authorization: Bearer <access-token-for-API-M-REST-API>" -d '{ "apikey" : "<API-Key-to-revoke>" }' -H "Content-Type: application/json" "https://localhost:9443/api/am/devportal/v3/applications/<application-ID>/api-keys/<key-type>/revoke"
    ```

    Example

    Info

    Refer to [Developer Portal - Revoke API Key](https://apim.docs.wso2.com/en/4.2.0/reference/product-apis/devportal-apis/devportal-v3/devportal-v3/#tag/API-Keys/paths/\~1applications\~1{applicationId}\~1api-keys\~1{keyType}\~1revoke/post) section for more info.

    The above command will return an empty 200 response.
5. Invoke the API with the same API key to verify whether the token has been revoked.
