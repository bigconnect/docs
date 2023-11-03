# Federating OAuth Applications

In the following document we will be explaining how to federate the OAuth applications using an external identity provider.

#### Prerequisites <a href="#prerequisites" id="prerequisites"></a>

1. Using the latest API-Manager distribution from [https://wso2.com/api-management/](https://wso2.com/api-management/)
2. Have an external IDP already configured. You can follow our [SSO Documentation](https://apim.docs.wso2.com/en/4.2.0/install-and-setup/setup/sso/okta-as-an-external-idp-using-oidc) to setup Okta as an external IDP

#### Configuration <a href="#configuration" id="configuration"></a>

1. Go to `<APIM_HOME>/repository/conf/identity/service-providers/` and open `default.xml`
2.  Comment out or remove the the `<LocalAuthenticatorConfigs ...` section and add the following.

    ```xml
        <FederatedIdentityProviders>
            <IdentityProvider>
                <!-- Name of the external IDP -->
                <IdentityProviderName>okta</IdentityProviderName>
                <IsEnabled>true</IsEnabled>
                <DefaultAuthenticatorConfig>
                    <FederatedAuthenticatorConfig>
                        <Name>OpenIDConnectAuthenticator</Name>
                        <IsEnabled>true</IsEnabled>
                    </FederatedAuthenticatorConfig>
                </DefaultAuthenticatorConfig>
            </IdentityProvider>
        </FederatedIdentityProviders>
    ```

    Note

    You can replace the FederatedAuthenticatorConfig name with your corresponding authenticator type of your IDP

    | Authenticator type | Config name                |
    | ------------------ | -------------------------- |
    | OpenId Connect     | OpenIDConnectAuthenticator |
    | SAML               | SAMLSSOAuthenticator       |
3. Now for the OAuth applications created using the Developer Portal, the above external IDP will be used to generate the access token.
