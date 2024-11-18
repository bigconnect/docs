# Azure AD

### Configuration

#### Create the application

Create the application using a new 'App Registration'

<figure><img src="https://files.readme.io/b6cb55f-image.png" alt=""><figcaption></figcaption></figure>

Add the application name

<figure><img src="https://files.readme.io/311e313-azure-add-application-2.png" alt=""><figcaption></figcaption></figure>

Ignore the Redirect URI for now.

#### Find your configuration details

**Configuration URL**

Visit 'Overview' > 'Endpoints'.

Use the **OpenID Connect metadata document** field as your Configuration URL.

> ### 📘
>
> Newer versions of Azure Portal may include a generic OpenID URL like: `https://login.microsoftonline.com/common/v2.0/.well-known/openid-configuration`
>
> In that case you will need to manually edit the URL and include the Azure Tenant ID: `https://login.microsoftonline.com/[AZURE_TENANT_ID]/.well-known/openid-configuration`

<figure><img src="https://files.readme.io/7fc1142-azure-configurationurl.png" alt=""><figcaption></figcaption></figure>

**Client ID**

Visit 'Overview'

Use the **Application (client) ID** field as your Client ID .

**Client Secret**

Visit 'Certificates & secrets', click on 'Add client secret', give your secret a description/expiry, and save.

View your newly created secret

<figure><img src="https://files.readme.io/c38ccd6-image.png" alt=""><figcaption></figcaption></figure>

Use the **Value** field as your Client Secret.

#### Finding your callback URL

Navigate to Settings -> Auth. Scroll down to where it says "OpenID Connect" and find the field labeled `Callback URL`.

<figure><img src="https://files.readme.io/5bb0daa-Screenshot_2024-02-23_at_11.52.38.png" alt=""><figcaption></figcaption></figure>

If you are using our cloud service, the URL contained within is exactly what you need.

For self-hosted users, it will likely display `http://localhost:10000/api/global/auth/oidc/callback`. This will need to be changed depending on how you are hosting it: `http[s]://[your-app.com]/api/global/auth/oidc/callback`. The parts that likely need to be changed are enclosed within square brackets.

#### Add your callback URL

Visit 'Authentication' and click 'Add a platform' -> 'Web'. Enter your callback URL.
