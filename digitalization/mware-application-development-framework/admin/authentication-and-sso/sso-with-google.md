# SSO with Google

Budibase currently supports Google OAuth in addition to the built-in authentication solution. To enable it, you need to provide OAuth credentials generated in the Google dashboard. You will need your Client ID, Client Secret, and Callback URL.

#### Generate Google Credentials

1. Go to the [Google Developer Console](https://console.cloud.google.com/cloud-resource-manager)
2. Open the hamburger menu, and select `APIs & Services`as shown below.

![](https://files.readme.io/19e0d6a-Screenshot\_2022-09-05\_at\_15.43.38.png)

3. Choose **Credentials** , then **Create Credentials** then **OAuth Client ID**
4. Choose **Web application** as the application type and enter a name for your own reference
5. Under **Authorized redirect URIs** enter the address to your Budibase installation 'callback address', for example:\
   `http://mybudibase.domain.com/api/global/auth/google/callback`
6.  After saving, copy the **Client ID** and **Client secret**\


    <figure><img src="https://files.readme.io/a1763c4-image.png" alt=""><figcaption></figcaption></figure>

**Optional - Google Sheets datasource configuration**

1. Add an additional **Authorized redirect URI** to your Client ID configuration. It will be the same as your original callback url, except it will have `/datasource` between `/auth` and `/google`, for example: `http://mybudibase.domain.com/api/global/auth/datasource/google/callback`

<figure><img src="https://files.readme.io/c3a6d67-image.png" alt=""><figcaption></figcaption></figure>

2. Navigate to the **Enabled APIs & Services** screen and click **+ Enable APIs and Services** at the top. This will take you to the Google Cloud API Library.

<figure><img src="https://files.readme.io/8ed9417-image.png" alt=""><figcaption></figcaption></figure>

3. Look for the **Google Sheets API** card in the API Library and click it.

<figure><img src="https://files.readme.io/2fa0e1e-image.png" alt=""><figcaption></figcaption></figure>

4. Click **Enable** and the Sheets API should be ready to use.

<figure><img src="https://files.readme.io/38939eb-image.png" alt=""><figcaption></figcaption></figure>

#### Configure Google Auth

Log in to your installation and visit the '**Auth**' section. Enter the Client ID and Client Secret generated above. The callback URL should be pre-filled based on the _Platform URL_ setting in the _Organisations_ menu.

<figure><img src="https://files.readme.io/c0bc205-image.png" alt=""><figcaption></figcaption></figure>

#### &#x20;Adding Users

After adding Google Auth you will need to visit the Users section and create an account for the user who will use Google to authenticate:

<figure><img src="https://files.readme.io/a2311c9-image.png" alt=""><figcaption></figcaption></figure>

<figure><img src="https://files.readme.io/715d5ac-image.png" alt=""><figcaption></figcaption></figure>

When the user verifies their email address, they will be listed in the **Users** menu and they will be able to log in using the Google button as shown below:

![](https://files.readme.io/073a5b3-image.png)
