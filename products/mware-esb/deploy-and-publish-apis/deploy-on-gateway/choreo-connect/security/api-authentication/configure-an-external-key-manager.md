# Configure an External Key Manager

### Via WSO2 API Manager Admin Portal[¶](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/choreo-connect/security/api-authentication/configuring-an-external-key-manager/#via-wso2-api-manager-admin-portal) <a href="#via-wso2-api-manager-admin-portal" id="via-wso2-api-manager-admin-portal"></a>

Choreo Connect provides the capability to configure external Key Managers through the API Manager Admin Portal. The issuer data is retrieved from the event hub at the startup, and updated as the changes are made from the Admin Portal.

#### Step 1 - Configure Choreo Connect with API Manager[¶](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/choreo-connect/security/api-authentication/configuring-an-external-key-manager/#step-1-configure-choreo-connect-with-api-manager) <a href="#step-1-configure-choreo-connect-with-api-manager" id="step-1-configure-choreo-connect-with-api-manager"></a>

Please refer [Configure Choreo Connect with API Manager](https://apim.docs.wso2.com/en/4.2.0/deploy-and-publish/deploy-on-gateway/choreo-connect/getting-started/deploy/cc-on-docker-with-apim-as-control-plane).

#### Step 2 - Add the external key manager to API Manager[¶](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/choreo-connect/security/api-authentication/configuring-an-external-key-manager/#step-2-add-the-external-key-manager-to-api-manager) <a href="#step-2-add-the-external-key-manager-to-api-manager" id="step-2-add-the-external-key-manager-to-api-manager"></a>

Please refer [Multiple Key Manager Support in WSO2 API Manager](https://apim.docs.wso2.com/en/4.2.0/administer/key-managers/overview) to configure the desired key manager.

Note

Please note that Choreo Connect only supports self-validation of JWT tokens from key managers. (JWT tokens will be validated against the issuer data.)

Important

Token services which are added from the `config.toml` file under `enforcer.security.tokenService` configuration will be overridden from the retrieved key manager configurations from the API Manager if the issuers are identical. Furthermore, if the corresponding key manager is removed from the API Manager admin portal, the token service added from the configuration will be used.

### Via the Choreo Connect Config File[¶](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/choreo-connect/security/api-authentication/configuring-an-external-key-manager/#via-the-choreo-connect-config-file) <a href="#via-the-choreo-connect-config-file" id="via-the-choreo-connect-config-file"></a>

When Choreo Connect runs as a standalone Gateway, the external Key Managers, Token Services or JWT issuers used for API authentication must be configured in the [config.toml](https://apim.docs.wso2.com/en/4.2.0/deploy-and-publish/deploy-on-gateway/choreo-connect/configurations/configuration-overview/#configurations-overview). To know what these parameters mean, you can go through the descriptions given under [Token Service in Enforcer Configurations](https://apim.docs.wso2.com/en/4.2.0/deploy-and-publish/deploy-on-gateway/choreo-connect/configurations/enforcer-configurations/#token-service). The following are the token services configured by default. The template with the default values can also be found in `config.toml.template` located together with `config.toml`.

```toml
# Issuer 1 - Resident Key Manager Issuer for Access tokens
[[enforcer.security.tokenService]]
  name="Resident Key Manager"
  issuer = "https://localhost:9443/oauth2/token"
  certificateAlias = "wso2carbon"
  jwksURL = "https://apim:9443/t/wso2.com/oauth2/jwks"
  validateSubscription = false
  consumerKeyClaim = "azp"
  certificateFilePath = "/home/wso2/security/truststore/wso2carbon.pem"

# Issuer 2 - Issuer for Enforcer test key
[[enforcer.security.tokenService]]
  name = "MGW"
  issuer = "https://localhost:9095/testkey"
  certificateAlias = "mgw"
  jwksURL = ""
  validateSubscription = false
  consumerKeyClaim = ""
  certificateFilePath = "/home/wso2/security/truststore/mg.pem"

# Issuer 3 - Issuer for API Manager Internal Key
[[enforcer.security.tokenService]]
  name = "APIM Publisher"
  issuer = "https://localhost:9443/publisher"
  validateSubscription = true
  certificateAlias = ""
  certificateFilePath = "/home/wso2/security/truststore/wso2carbon.pem"

# Issuer 4 - Issuer for API Manager API Key
[[enforcer.security.tokenService]]
    # Provide unique name for the JWT issuer
    name = "APIM APIkey"
    validateSubscription = true
    # Alias name given in Enforcer truststore for the public certificate of the JWT issuer
    certificateAlias = "apikey_certificate_alias"
    # Certificate Filepath within Enforcer
    certificateFilePath = "/home/wso2/security/truststore/wso2carbon.pem"
```
