# SSO with Auth0

### Configuration

#### Create the application

Create the application in your Applications list.

<figure><img src="https://files.readme.io/c2c0abb-auth0-create-application.png" alt=""><figcaption></figcaption></figure>

Select the 'web' tab, then click create.

<figure><img src="https://files.readme.io/f99b2ea-Screenshot_2024-04-03_at_08.35.49.png" alt=""><figcaption></figcaption></figure>

#### Find your configuration details

Visit the 'Settings' tab in your application.

**Configuration URL**

Use the **Domain** field to construct your Configuration URL. e.g. `https://{domain}/.well-known/openid-configuration`

**Client ID**

Use the **Client ID** field as your Client ID.

**Client Secret**

Use the **Client Secret** field as your Client Secret.

#### Finding your callback URL

Navigate to Settings -> Auth. Scroll down to where it says "OpenID Connect" and find the field labeled `Callback URL`.

<figure><img src="https://files.readme.io/5bb0daa-Screenshot_2024-02-23_at_11.52.38.png" alt=""><figcaption></figcaption></figure>

If you are using our cloud service, the URL contained within is exactly what you need.

For self-hosted users, it will likely display `http://localhost:10000/api/global/auth/oidc/callback`. This will need to be changed depending on how you are hosting it: `http[s]://[your-app.com]/api/global/auth/oidc/callback`. The parts that likely need to be changed are enclosed within square brackets.

#### Add your callback URL

Scroll down to the 'Application URIs' section and enter your callback URL

<figure><img src="https://files.readme.io/0a31fcd-34b880b-auth0-callback.png" alt=""><figcaption></figcaption></figure>
