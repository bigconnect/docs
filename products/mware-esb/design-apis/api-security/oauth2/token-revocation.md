# Token Revocation

### Revoking access tokens[¶](https://apim.docs.wso2.com/en/latest/design/api-security/oauth2/token-revocation/#revoking-access-tokens) <a href="#revoking-access-tokens" id="revoking-access-tokens"></a>

After issuing an access token, a user or an admin can revoke it in case of theft or a security violation. You can do this by calling the Revoke API using a utility like cURL. The Revoke API's endpoint URL is `http://localhost:8280/revoke`.

Note

You can also revoke refresh tokens. For more information, see [Revoking a refresh token](https://apim.docs.wso2.com/en/4.2.0/design/api-security/oauth2/grant-types/refresh-token-grant/#revoking-a-refresh-token).

The parameters required to invoke the following API are as follows:

* `token` - The token to be revoked
*   `<base64 encoded (clientId:clientSecret)>` - Use a base64 encoder to encode your client ID and client secret using the following format. WSO2 does not recommend the use of online base64 encoders for this purpose.

    `<clientId>:<clientSecret>` Thereafter, enter the encoded value for this parameter.
*   `token_type_hint` = This parameter is **optional**. If you do not specify this parameter, then WSO2 Identity Server (WSO2 IS) will search in both key spaces (access and refresh) and if it finds a matching token then it will be revoked. Therefore, if this parameter it not specified the token revocation process takes longer. However, if you specify this parameter then WSO2 IS only searches in the respective token key space, hence the token revocation process is much faster.

    Format

    ```
        curl -k -v -d "token=<ACCESS_TOKEN_TO_BE_REVOKED>" -H "Authorization: Basic <base64 encoded (clientId:clientSecret)>" -H "Content-Type: application/x-www-form-urlencoded" https://localhost:9443/oauth2/revoke
        curl -k -v -d "token=<ACCESS_TOKEN_TO_BE_REVOKED>&token_type_hint=<access_token_or_refresh_token>" -H "Authorization: Basic <base64 encoded (clientId:clientSecret)>" -H "Content-Type: application/x-www-form-urlencoded" https://localhost:9443/oauth2/revoke
    ```

    ExamplesResponse

Revoking access tokens obtained with a JWT grant

If you obtained an access token with the JWT grant type, you do not have to provide the client secret to revoke it. The sample cURL command to revoke an access token with JWT grant is given below.

```java
curl -X POST -H "Content-Type: application/x-www-form-urlencoded;charset=UTF-8" -d "token=<ACCESS_TOKEN_TO_BE_REVOKED>&token_type_hint=access_token&client_id=<CLIENT_ID>" http://localhost:8243/revoke
```
