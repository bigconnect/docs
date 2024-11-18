# SSO with Okta

### Configuration

#### Create the application

Create the application in your Applications list.

<figure><img src="https://files.readme.io/0fb65cd-okta-create.png" alt=""><figcaption></figcaption></figure>

Select `OIDC - OpenID Connect` and `Web Application`

<figure><img src="https://files.readme.io/ab388e0-okta-create-oidc.png" alt=""><figcaption></figcaption></figure>

Enter your application name on the next screen and save.

#### Find your configuration details

Visit the 'General' tab in your application.

**Client ID**

Use the **Client ID** field as your Client ID.

**Client Secret**

Use the **Client Secret** field as your Client ID.

**Configuration URL**

Visit the 'Sign On' tab in your application.

<figure><img src="https://files.readme.io/d528712-okta-url.png" alt=""><figcaption></figcaption></figure>

Use the **Issuer** field to construct your Configuration URL  e.g. `https://{Issuer}/.well-known/openid-configuration`

#### Add your callback URL

Visit the 'General' tab in your application, scroll down and edit the 'General Settings' section. Enter your callback URL

<figure><img src="https://files.readme.io/feb845f-okta-callback.png" alt=""><figcaption></figcaption></figure>
