# Creating an OAuth2 Application

When you have an application that can use OAuth 2.0 for authorization, you must register that application so Liferay can recognize it. Do this by accessing _Control Panel_ → _Security_ → _OAuth2 Administration_:

1. Click the _Add_ (![add](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png)) button.
2. Fill out the form ([description below](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/using-oauth2/creating-oauth2-applications#oauth2-administration-reference)).
3.  Click _Save_ to save the application.

    ![Adding an application registers it so users can authorize access to their data.](https://resources.learn.liferay.com/images/dxp/latest/en/headless-delivery/using-oauth2/creating-oauth2-applications/images/01.png)

Excellent! Now you know how to add OAuth2 authorization for your application to Liferay Portal! Next, you must define scopes of user data the application can access.

### [OAuth2 Administration Reference](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/using-oauth2/creating-oauth2-applications#oauth2-administration-reference) <a href="#oauth2-administration-reference" id="oauth2-administration-reference"></a>

**Name:** Give the application a recognizable title.

**Website URL:** Add a link to the application’s website.

**Callback URIs:** Enter at least one (line-separated) URI where users should be redirected after they authorize (or refuse to authorize) access to their accounts. This should link to a handler for whichever Allowed Authorization Types you support (see below).

**Client Profile:** Choose a template that filters the authorization types that are appropriate (secure) for that profile. For example, if your application is a web application, choose _Web Application_, and these authorization types are available and selected automatically: Authorization Code, Client Credentials, Refresh Token, and Resource Owner Password Credentials. These are OAuth 2 “flows” documented in the [OAuth2 RFC 6749 Standards Document](https://tools.ietf.org/html/rfc6749). If you want to select authorization types manually, select _Other_.

**Allowed Authorization Types:** Select the defined OAuth 2 [protocol flows](https://tools.ietf.org/html/rfc6749#section-1.2) your application supports. Several common combinations are defined for you in the various Client Profiles above.

**Client Credentials User:** Select the user whose credentials should be used for the Client Credentials authorization type. This only works for this type.

**Trusted Application:** For authorization code or PKCE flows, check this box to consent to the access token automatically, without requiring the user to allow it.

**Remember Device:** For authorization code or PKCE flows, check this box to allow users to grant access automatically on their chosen devices when access tokens expire.

After you save the form, it reappears with additional fields:

**Client ID:** The system generates this for you; it’s an identifier for your application, so that DXP knows what application is being authorized to access user data.

**Client Secret:** Click the _pencil_ icon to generate a client secret. The secret identifies the client during the authorization process (see figure 1 above). Not all client profiles require a client secret, because some are incapable of keeping it secret! This is when the aforementioned PKCE code challenge and verifier is needed.

**Icon:** Upload an icon that your application’s users identify with your application. This is displayed on the authorization screen.

**Privacy Policy URL:** Add a link to your application’s privacy policy.

**Token Introspection:** Allow your application to retrieve metadata from the token by requesting it from DXP. This implements [RFC 7662](https://tools.ietf.org/html/rfc7662).
