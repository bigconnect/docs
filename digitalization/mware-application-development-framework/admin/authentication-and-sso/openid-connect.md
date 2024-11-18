# OpenID Connect

OpenID Connect (OIDC) is a simple identity layer built on top of the OAuth 2.0 protocol, which allows clients to verify the identity of an end-user based on the authentication performed by an authorization server or identity provider (IdP), as well as to obtain basic profile information about the end-user.

<figure><img src="https://files.readme.io/afb9eda-oidc.png" alt=""><figcaption></figcaption></figure>

### Setup

To enable OIDC for your IdP follow the setup steps below.

#### Configure your IdP

Any provider that supports the OIDC protocol can be integrated into Mware Application Development Framework, you may even implement your own.

Guides on the most common providers are documented for you, you can find them in the sidebar. These include Auth0, Azure AD, Okta, and more.

**Callback URL**

* During your configuration you will need to enter the Callback URL for your installation.
* For example: `https://{your-host.com}/api/admin/auth/oidc/callback`

#### Configure OIDC

To configure an OIDC integration visit the `Auth` section of the builder.

Fill in the following options from your IdP:

* **Config URL**
  * URL where the system can find the [OpenID Provider Configuration Document](https://openid.net/specs/openid-connect-discovery-1\_0.html#ProviderConfig)
  * For example: `https://{your-identity-provider.com}/.well-known/openid-configuration`
* **Client ID**
  * Your unique ID issued by your IdP
* **Client Secret**
  * Your unique secret issued by your IdP

Save the configuration to enable OIDC on your login page.

#### Customize your login

Use the login configuration options to customize the OIDC login button.

![](https://docs.budibase.com/.gitbook/assets/oidc-login-form.png)

Fill in either of the following:

* **Name**
  * The name on the login button. This will be substituted at `Sign in with {name}`
* **Icon**
  * The icon on the login button. Choose from:
    * One of the default icons
    * Upload a custom icon

### Important Information

Some additional details on the OIDC integration are highlighted below.

**User provisioning**

Unlike the Google integration which requires a local user account to exist in advance, OIDC users are created in the system automatically when they log in for the first time. It is important that only the users you wish to access the system have been assigned to the application configured in your IdP.

You may still use email onboarding to create an account for a user in advance, provided the email matches the user's email in your IdP.
