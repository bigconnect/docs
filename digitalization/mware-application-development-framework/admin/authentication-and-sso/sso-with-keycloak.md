# SSO with Keycloak

### Configuration

#### Add a Client

Create a new client by visiting the 'Clients' pane from the left-hand side of your Keycloak admin console. Then give it an ID and select 'openid-connect' from the dropdown. Alternatively, bypass this step if you have a client already created.

<figure><img src="https://files.readme.io/ad46b3c-Keycloak_-_Create_Client.png" alt=""><figcaption></figcaption></figure>

#### Configure the Client

In the Client configuration screen (accessed by clicking on the Client ID within the Clients pane), change the 'Access Type' dropdown to be 'Confidential'.

<figure><img src="https://files.readme.io/5208426-Keycloak_-_Access_Type.png" alt=""><figcaption></figcaption></figure>

Next add the Callback URL to the 'Valid Redirect URIs' field ands then click 'Save'

<figure><img src="https://files.readme.io/e0172e2-Keycloak_-_Redirect_URI.png" alt=""><figcaption></figcaption></figure>

#### Find your configuration details

**Client ID**

The Client ID field should be the same ID as you gave the Client in Keycloak earlier.

<figure><img src="https://files.readme.io/6e900e0-Keycloak_-_Client_ID.png" alt=""><figcaption></figcaption></figure>

**Client Secret**

You can find the Client Secret from the Credentials tab in the Client Configuration screen

<figure><img src="https://files.readme.io/8bc7ee8-Keycloak_-_Client_Secret.png" alt=""><figcaption></figcaption></figure>

**Configuration URL**

Visit the 'Realms' pane from the left hand side of your Keycloak admin console. Then click on the OpenID Endpoint Configuration link and copy the URL that opens.

<figure><img src="https://files.readme.io/d56c61c-Keycloak_-_Well-known_configuration.png" alt=""><figcaption></figcaption></figure>

<figure><img src="https://files.readme.io/8e189ee-Keycloak_-_URL.png" alt=""><figcaption></figcaption></figure>
