# SSO with OneLogin

### Configuration

#### Create the application

Create the application in your Applications list.

<figure><img src="https://files.readme.io/3e476ee-onelogin-create.png" alt=""><figcaption></figcaption></figure>

Type 'openid connect' and select 'Openid Connect (OIDC)'

<figure><img src="https://files.readme.io/a333c2f-onelogin-openid.png" alt=""><figcaption></figcaption></figure>

Give your application a name and save

<figure><img src="https://files.readme.io/d828fc5-onelogin-add-name.png" alt=""><figcaption></figcaption></figure>

Visit the 'SSO' tab and choose

* 'Application Type' = 'Web'
* 'Authentication Method' = 'POST'

<figure><img src="https://files.readme.io/f99daab-onelogin-type.png" alt=""><figcaption></figcaption></figure>

#### Find your configuration details

Visit the 'SSO' tab in your application.

<figure><img src="https://files.readme.io/6cefc2c-onelogin-details.png" alt=""><figcaption></figcaption></figure>

**Client ID**

Use the **Client ID** field as your Client ID.

**Client Secret**

Use the **Client Secret** field as your Client ID.

**Configuration URL**

Use the **Issuer URL** field to construct your Configuration URL, e.g. `https://{Issuer URL}/.well-known/openid-configuration`

#### Add your callback URL

Visit the 'Configuration' tab in your application and enter your callback URL

<figure><img src="https://files.readme.io/a11cd57-onelogin-callback.png" alt=""><figcaption></figcaption></figure>
