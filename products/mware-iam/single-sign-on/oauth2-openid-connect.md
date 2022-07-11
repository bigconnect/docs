# OAuth2-OpenID Connect

OAuth provides a method for clients to access server resources on behalf of a resource owner (such as a different client or an end-user). It also provides a process for end-users to authorize third-party access to their server resources without sharing their credentials (typically, a username and password pair), using user-agent redirections.

OpenID Connect is a simple identity layer on top of the OAuth 2.0 protocol. It allows Clients to verify the identity of the End-User based on the authentication performed by an Authorization Server, as well as to obtain basic profile information about the End-User in an interoperable and REST-like manner.

OpenID Connect allows clients of all types, including Web-based, mobile, and JavaScript clients, to request and receive information about authenticated sessions and end-users. The specification suite is extensible, allowing participants to use optional features such as encryption of identity data, discovery of OpenID Providers, and session management, when it makes sense for them.

## Configuring OAuth2-OpenID Connect Single-Sign-On <a href="#configuring-oauth2-openid-connect-single-sign-on" id="configuring-oauth2-openid-connect-single-sign-on"></a>

[OAuth 2.0](https://oauth.net/2/) has three main phases. They are:

* Requesting an Authorization Grant
* Exchanging the Authorization Grant for an Access Token&#x20;
* Accessing the resources using the obtained Access Token

[OpenID Connect](http://openid.net/connect/) is another identity layer on top of OAuth 2.0. OAuth applications can get authentication event information over the IDToken and can get the extra claims of the authenticated user from the OpenID Connect UserInfo endpoint.

To enable OAuth support for your client application, you must first register your application. Follow the instructions below to add a new application.

Let's get started to configure the service provider you created!

1. Expand the **Inbound Authentication Configuration** section and then expand **OAuth/OpenID Connect Configuration.** Click **Configure**.
2. Fill in the form that appears. For the **Allowed Grant Types** you can disable the ones you do not require or wish to block.\
   ![](<../../../.gitbook/assets/image (714).png>)
3. Click **Add**. Note that client key and client secret get generated.\
   ![](<../../../.gitbook/assets/image (702).png>)

* **OAuth Client Key** - This is the client key of the service provider, which will be checked for authentication by the Identity Server before providing the access token.
* **OAuth Client Secret** - This is the client secret of the service provider, which will be checked for authentication by the Identity Server before providing the access token. Click the **Show** button to view the exact value of this.
* **Actions -**
  * **Edit:** Click to edit the OAuth/OpenID Connect Configurations
  * **Revoke:** Click to revoke (deactivate) the OAuth application. This action revokes all tokens issued for this application. In order to activate the application, you have to regenerate the consumer secret.
  * **Regenerate Secret:** Click to regenerate the secret key of the OAuth application.
  * **Delete:** Click to delete the OAuth/OpenID Connect Configurations.

After updating the configuration, make sure to restart the server for the changes to be applied on IAM Server.

## Configuring OpenID Connect Single Logout <a href="#configuring-openid-connect-single-logout" id="configuring-openid-connect-single-logout"></a>

The [OpenID Connect Session Management specification](http://openid.net/specs/openid-connect-session-1\_0.html) , provides a way for a Relying Party (RP) to monitor the login status of an end user with an OpenID Connect Provider (OP) so that the RP application can log-out any end users that have logged out of the OP (i.e., the IAM Server). For instance, if there are two RP applications relying on IAM Server, when an end user logs-out of one of the applications, he/she can be automatically logged out of the other one as well.

### How it works <a href="#how-it-works" id="how-it-works"></a>

OpenID Connect session management works with two hidden iframes where both reside at the RP. One is from the RP itself and the other is from the OP. When authenticating, the OP sends an iframe to the RP. The RP embed this OP-provided iframe into the RP. The RP checks the session state via the RP iframe by continuously polling the embedded OP provided iframe, without causing network traffic. Thereby, the RP is notified when the session state of the end-user has changed. The flow of this is as follows.

1. The RP iframe polls the OP iframe for a session status.
2. The OP iframe sends back a message (by using HTML5 Winodw.postMessage()) about the session state as 'changed, 'unchanged' or 'error'.
3. If the session state is 'changed', the RP sends a passive request for re-authentication.
4. If the end user has logged out from the OP, the RP will receive an authentication failure message along with a new session state value. The RP handles this as a end user logout.
5. If the end user has not logged out, the RP will receive a successful authentication response along with a new session state value.
