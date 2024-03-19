# Using Authentication Verifiers

Authentication Verifiers authenticate remote invocations of Liferay Portal’s API in a centralized and extensible way. They have two main responsibilities:

1. Verify provided credentials using registered `AuthVerifier` instances
2. Create portal authorization contexts based on verification results

If no available `AuthVerifier` can determine that the provided credentials match a user account, an authorization context supporting non-authenticated access is created for a guest user. This allows each API to expose only a single API endpoint. In contrast, legacy (prior to 6.2) versions of Liferay Portal exposed two API endpoints for each API: the `/api/endpoint` URI was for non-authenticated access and the URI `/api/secure/endpoint` was for authenticated access.

There are built-in `AuthVerifier` implementations for the most common situations, such as when remote clients use HTTP Basic or HTTP Digest authentication, send credentials in request parameters, send authenticated `JSESSIONID`s, or use shared secrets to establish trust. Other `AuthVerifier` implementations can be deployed as modules containing implementations of the `AuthVerifier` interface that are registered as services in the OSGi runtime.

IMPORTANT

The authentication verification layer’s focus is on verifying authentication, not on providing credentials. It does NOT issue tokens, credentials, or display Sign In portlets. Instead, the layer verifies existing credentials and authenticated sessions and is a complement to authentication endpoints. To ensure backwards compatibility the default implementations support requests providing user name and password credentials. Thus, the authentication verification layer stands on the border between authentication and authorization.

### [Authentication Verification Process](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/securing-liferay/securing-web-services/using-authentication-verifiers#authentication-verification-process) <a href="#authentication-verification-process" id="authentication-verification-process"></a>

To configure Authentication Verifiers, go to _Control Panel_ → _Configuration_ → _System Settings_ → _Security_ → _API Authentication_.

![The Authentication Verifier Configuration Screen](https://resources.learn.liferay.com/images/dxp/latest/en/installation-and-upgrades/securing-liferay/securing-web-services/using-authentication-verifiers/images/01.png)

#### [Step 1: Verify Request Credentials](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/securing-liferay/securing-web-services/using-authentication-verifiers#step-1-verify-request-credentials) <a href="#step-1-verify-request-credentials" id="step-1-verify-request-credentials"></a>

The layer uses the chain of responsibility design pattern to support both built-in and third party `AuthVerifier` implementations. Each `AuthVerifier` can provide configurations where it specifies mapped URLs and other properties.

Each incoming request is matched against all registered `AuthVerifier`s to select the final list of `AuthVerifier`s that is used to process the request. It’s the responsibility of each `AuthVerifier` to verify the incoming request credentials.

#### [Step 2: Create an Authorization Context](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/securing-liferay/securing-web-services/using-authentication-verifiers#step-2-create-an-authorization-context) <a href="#step-2-create-an-authorization-context" id="step-2-create-an-authorization-context"></a>

When a request is processed by all matching `AuthVerifier`s, Liferay DXP creates an authorization context for the resolved user.

This encompasses setting the `HttpServletRequest` `remoteUser` to return the resolved user ID setting `ThreadLocal`s to the resolved user.

The resolved user can be the user returned by one of the `AuthVerifier` instances or a guest user if no instance was able to verify the provided credentials.

### [Configuring Built-In Authentication Verifiers](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/securing-liferay/securing-web-services/using-authentication-verifiers#configuring-built-in-authentication-verifiers) <a href="#configuring-built-in-authentication-verifiers" id="configuring-built-in-authentication-verifiers"></a>

`AuthVerifier`s are created by developers, and are processed automatically as long as they’re registered in the OSGi runtime. This layer and surrounding processes are provided by the `AuthVerifierFilter` class that implements the `javax.servlet.Filter` interface. Configuration for Auth Verifiers that ship with the product include:

* [Basic Auth Header](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/securing-liferay/securing-web-services/using-authentication-verifiers#basic-auth-header)\*
* [Digest Authentication](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/securing-liferay/securing-web-services/using-authentication-verifiers#digest-auth-header)
* [HTTP Tunnel Extender](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/securing-liferay/securing-web-services/using-authentication-verifiers#http-tunnel-extender)
* [Image Request](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/securing-liferay/securing-web-services/using-authentication-verifiers#image-request-authentication-verifier)
* [Portal Sessions](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/securing-liferay/securing-web-services/using-authentication-verifiers#portal-sessions-auth-verifiers)\*
* [Request Parameter](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/securing-liferay/securing-web-services/using-authentication-verifiers#request-parameter)
* [Tunnel Auth](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/securing-liferay/securing-web-services/using-authentication-verifiers#tunnel-auth)

> \*Enabled by default and can be used to access remote APIs out-of-the-box.

#### [Basic Auth Header](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/securing-liferay/securing-web-services/using-authentication-verifiers#basic-auth-header) <a href="#basic-auth-header" id="basic-auth-header"></a>

This Auth Verifier allows the remote client to authenticate using [HTTP Basic Authentication](https://en.wikipedia.org/wiki/Basic\_access\_authentication). Configure it by providing URL paths that should be authenticated this way. When _Force Basic Authentication_ field is checked then HTTP Basic Authentication is required.

The default URLs are `/api/*,/xmlrpc*` for web services. The mapping excludes `/api/liferay*` to prevent accessing `TunnelServlet`. For more information please see Tunnel Authentication Verifiers.

#### [Digest Auth Header](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/securing-liferay/securing-web-services/using-authentication-verifiers#digest-auth-header) <a href="#digest-auth-header" id="digest-auth-header"></a>

This Auth Verifier allows the remote client to authenticate using [HTTP Digest Authentication](https://en.wikipedia.org/wiki/Digest\_access\_authentication). Configure it by providing URL paths that should be authenticated this way. When Force Digest Authentication field is checked then HTTP Basic Authentication is required.

This Auth Verifier is not enabled by default.

#### [HTTP Tunnel Extender](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/securing-liferay/securing-web-services/using-authentication-verifiers#http-tunnel-extender) <a href="#http-tunnel-extender" id="http-tunnel-extender"></a>

As Liferay embraced modularity, this extender was written to enable modules to be part of `TunnelServlet`. It maps `TunnelServlet` and `TunnelingServletAuthVerifier` to the module servlet context. Modules with `Http-Tunnel` in the manifest can make use of the Tunnel Servlet, and can expose the API via `/o/_module_/api/liferay/do`.

Configure it by setting client IP addresses allowed to tunnel. For more information, please see [the properties documentation](https://learn.liferay.com/reference/latest/en/dxp/propertiesdoc/portal.properties.html#HTTP%20Tunneling) as well as [remote staging](https://learn.liferay.com/web/guest/w/dxp/site-building/publishing-tools/staging/configuring-remote-live-staging).

Note that this is not a recommended way to export remote APIs; it’s far better to expose remote services using JAX-RS or Liferay JSON Web Service technologies.

#### [Image Request Authentication Verifier](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/securing-liferay/securing-web-services/using-authentication-verifiers#image-request-authentication-verifier) <a href="#image-request-authentication-verifier" id="image-request-authentication-verifier"></a>

When connected to LibreOffice/OpenOffice, the Office process must download images from Liferay Portal to render docs with images. To do this, a [JWT Token](https://jwt.io/) is created to access the images securely.

Configure this by setting the Hosts Allowed, URLs included, and URLs excluded if necessary.

This Auth Verifier is not enabled by default.

#### [Portal Sessions Auth Verifiers](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/securing-liferay/securing-web-services/using-authentication-verifiers#portal-sessions-auth-verifiers) <a href="#portal-sessions-auth-verifiers" id="portal-sessions-auth-verifiers"></a>

Enables JavaScript in a browser to access Liferay JSON Web Services using an existing portal session.

In the default configuration, the URLs included field shields access to the legacy JSON remote services layer: `/api/json*,/api/jsonws*,/c/portal/json_service*`.

#### [Request Parameter Auth Verifiers](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/securing-liferay/securing-web-services/using-authentication-verifiers#request-parameter-auth-verifiers) <a href="#request-parameter-auth-verifiers" id="request-parameter-auth-verifiers"></a>

For backwards compatibility with `RequestParameterAutoLogin` you can authenticate and access portal endpoints with credentials inside HTTP request parameters `parameterAutoLoginLogin` and `parameterAutoLoginPassword`.

This Auth Verifier is not enabled by default.

#### [Tunnel Authentication Verifiers](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/securing-liferay/securing-web-services/using-authentication-verifiers#tunnel-authentication-verifiers) <a href="#tunnel-authentication-verifiers" id="tunnel-authentication-verifiers"></a>

`TunnelServlet` is a legacy remote API endpoint mapped at `/api/liferay/do` to provide access to the portal remote services. The Tunnel Auth Verifier allows trusted remote clients authenticated access using any user ID provided, on behalf of the user.

An example of a trusted remote client is the Staging remote publishing feature.

Trusted remote clients authenticate using a shared secret stored in the portal property `tunneling.servlet.shared.secret`. The default value is empty and forbids all access.

Even though the default configuration is enabled by default, access is limited to localhost only. Configure it by setting client IP addresses allowed to tunnel. For more information, please see [the properties documentation](https://learn.liferay.com/reference/latest/en/dxp/propertiesdoc/portal.properties.html#HTTP%20Tunneling) as well as [remote staging](https://learn.liferay.com/web/guest/w/dxp/site-building/publishing-tools/staging/configuring-remote-live-staging).

\
