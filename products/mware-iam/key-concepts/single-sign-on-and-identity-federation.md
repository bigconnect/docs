# Single Sign-On and Identity Federation

### Introducing single sign-on <a href="#introducing-single-sign-on" id="introducing-single-sign-on"></a>

Single sign-on (SSO) is one of the key features of the MWARE Identity Server that enables users to provide their credentials once and obtain access to multiple applications. The users are not prompted for their credentials when accessing each application until their session is terminated. Additionally, the user can access all these applications without having to log into each and every one of them individually. So, if users log into application A, for example, they would automatically have access to application B as well for the duration of that session without having to re-enter their credentials.

The following are some of the advantages you can have with SSO:

* Users need only a single username/password pair to access multiple services. Thus they do not have the issue of remembering multiple username/password pairs.
* Users are authenticated only once at the identity provider and then they are automatically logged into all services within that "trust-domain". This process is more convenient to users since they do not have to provide their username/password at every service provider.
* Service providers do not have the overhead of managing user identities, which is more convenient for them.
* User identities are managed at a central point. This is more secure, less complex and easily manageable.

### How single sign-on works <a href="#how-single-sign-on-works" id="how-single-sign-on-works"></a>

To understand how single sign-on works, it is useful to first examine how authentication works in a non SSO environment. The following diagram depicts this flow.

![Authentication in a non SSO environment](https://is.docs.wso2.com/en/latest/assets/img/getting-started/authentication-in-non-sso.png)

In the above figure, the users access **Application 1** first and then **Application 2** using the same browser. The user is authenticated in **Application 1** first and can access this application. When the users attempt to access **Application 2**, they must enter their credentials again and get access to **Application 2** separately. There is no way for **Application 2** to access the cookie stored in the browser for **Application 1** and authenticate the user by that means.

The following diagram depicts how this story differs in a SSO environment.

![Authentication in a SSO environment](https://is.docs.wso2.com/en/latest/assets/img/getting-started/authentication-in-sso.png)

In this second figure, the users access **Application 1** first followed by **Application 2**. Once again, the users use the same browser to access these applications. The users are redirected to the MWARE Identity Server (or any authentication server that is configured for this task). If the users are already logged in to the Identity Server, the user is authenticated in **Application 1** via an authentication token sent by the Identity Server to **Application 1**. When **Application 2** is accessed, the same thing happens and the users are redirected to the Identity Server and authenticated. This is how SSO works.

### SSO in reality <a href="#sso-in-reality" id="sso-in-reality"></a>

Single sign-on is widely used in web technologies. Google is one of the best examples. Try this simple exercise.

1. Visit [www.google.com](http://www.google.com/) from your web browser.
2. Click the **SIGN IN** button on the top right of the page.
3. Once you sign in, you are redirected to [https://accounts.google.com/ServiceLogin](https://accounts.google.com/ServiceLogin) . There you are requested to enter your Username and Password. Enter your Google credentials there.
4. Once you enter your Username and Password, you are directed back to [www.google.com](http://www.google.com/) where you started.
5. Next visit [www.gmail.com](http://www.gmail.com/), the Google mail server.
6. Notice that you are automatically signed in and you directly access your Gmail Inbox. You did not have to enter your Username and Password at Gmail.
7. In addition to that; now try [www.youtube.com](http://www.youtube.com/).
8.  You are automatically signed in. You do not have to enter your username and password at YouTube.

    Tip

    Notice the URL of the web browser. Each time you access an application, you see that you are being redirected to [https://accounts.google.com/ServiceLogin](https://accounts.google.com/ServiceLogin) and returned immediately to the website.

Single Sign-On (SSO) allows you to sign in only once but provides access to multiple resources without having to re-enter your username and password.

### Supported protocols for SSO <a href="#supported-protocols-for-sso" id="supported-protocols-for-sso"></a>

The following are the supported protocols for agent-based single sign-on.

* SAML 2.0
* WS-Federation
* OpenID Connect
* WS-Trust

The following sections in this topic expand on these protocols and provide details pertaining to them.

### SAML 2.0 SSO <a href="#saml-20-sso" id="saml-20-sso"></a>

SAML stands for Security Assertion Markup Language, which is an XML-based data format for exchanging authentication and authorization data between an identity provider and a service provider. The single most important requirement that SAML addresses is web browser single sign-on (SSO). Three main roles are defined in SAML Specification.

* **The Principal** : This is typically the user who requires a service from a service provider entity.
* **The Identity Provider** : The SAML authority that provides the identity assertion to authenticate a principal.
* **The Service Provider** : The SAML consumer that provides the service for principals.

The main use case scenario covered by SAML is the principal (the user) requesting access to a resource or service from the service provider. Then the service provider, using SAML, communicates with the identity provider to obtain identity assertion. The service provider makes the access control decision, depending on this assertion.

SAML 2.0 is the latest version of SAML, which uses security tokens containing assertions to pass information about a user between an identity provider and a service provider.

SAML 2.0 provides five main specifications:

* Core
* Bindings
* Profiles
* Metadata
* Conformances

#### **SAML 2.0 web browser-based SSO profile**

SAML 2.0 Web Browser based SSO profile is defined under the SAML 2.0 Profiles specification.

In a web browser based SSO system, the flow can be started by the user either by attempting to access a service at the service provider or by directly accessing the identity provider itself.

**If the user accesses a service at a service provider:**

1. The service provider determines which identity provider to use (this is the case when there are multiple identity providers. SAML identity provider discovery profile may be used).
2. The service provider generates a SAML message and then redirects the web browser to the identity provider along with the message.
3. Identity provider authenticates the user.
4. The identity provider generates a SAML message and then redirects the web browser back to the service provider.
5. The service provider processes the SAML message and decides to grant or deny access to the user.

If the user accesses the identity provider directly, then only the steps 3, 4 and 5 are in the flow.

The following diagram depicts this flow.

![SAML 2.0 authentication flow](https://is.docs.wso2.com/en/latest/assets/img/getting-started/saml-authentication-flow.png)

The message MUST contain an element that uniquely identifies the service provider who created the message. Optionally, the message may contain elements such as Issuer, NameIDPolicy, etc. More information regarding the message can be found in [SAML Core Specification](http://www.oasis-open.org/committees/download.php/35711/sstc-saml-core-errata-2.0-wd-06-diff.pdf) .

#### **Identity provider initiated SSO**

To initiate IdP Initiated SSO you need to perform a HTTP GET/POST to the following URL (assume the registered service provider issuer ID is [foo.com](http://foo.com/) ). The SAML protocol message can be encoded using Base64 encoding.

[https://localhost:9443/samlsso?spEntityID=foo.com](https://localhost:9443/samlsso?spEntityID=foo.com)

This request will authenticate and redirect the user to the registered Assertion Consumer URL. You can use `acs` query parameter in the request to specify the Assertion Consumer URL that the browser should be redirected to after the authentication is successful.

* If the `acs` query parameter is not present in the request, the Identity Server sends the response to default ACS URL of the service provider.
* If the `acs` parameter is present and the value of that parameter matches with any of the registered ACS URLs of the service provider, then the Identity Server sends the response to the matched one.

[https://localhost:9443/samlsso?spEntityID=foo.com\&acs=http://localhost:8080/foo.com/my-home.jsp](https://localhost:9443/samlsso?spEntityID=foo.com\&acs=http://localhost:8080/foo.com/my-home.jsp)

Optionally, you can send a `RelayState` parameter as follows:

[https://localhost:9443/samlsso?spEntityID=foo.com\&RelayState=http://localhost:8080/foo.com/my-home.jsp](https://localhost:9443/samlsso?spEntityID=foo.com\&RelayState=http://localhost:8080/foo.com/my-home.jsp)

The `RelayState` parameter must be URL encoded. This request will authenticate and redirect the user to the URL in the RelayState parameter itself.

{% hint style="info" %}
Either you could have service provider initiated SSO only, or service provider initiated SSO and identity provider initiated SSO. You cannot have only identity provider initiated SSO. By design, service provider initiated SSO is more restrictive and secure. If a service provider is allowed to do identity provider initiated SSO, it would automatically imply that this service provider is allowed to do service provider initiated SSO as well.
{% endhint %}

{% hint style="info" %}
See [SAML 2.0 specification documentation](https://docs.oasis-open.org/security/saml/v2.0/saml-core-2.0-os.pdf) for more information.
{% endhint %}

### WS-Federation <a href="#ws-federation" id="ws-federation"></a>

WS-Federation (Web Services Federation) describes the management and brokering of trust relationships and security token exchange across Web services and organizational boundaries. WS-Federation is a part of the larger WS-Security framework. For example, WS-Federation builds on the Security Token Service (STS) by providing mechanisms that facilitate interactions. In the WS-Federation Model, an Identity Provider is a Security Token Service (STS).

{% hint style="info" %}
See [WS-Federation specification documentation](http://docs.oasis-open.org/wsfed/federation/v1.2/ws-federation.html) for more information.
{% endhint %}

### OpenID Connect <a href="#openid-connect" id="openid-connect"></a>

OpenID Connect is a simple identity layer on top of the OAuth 2.0 protocol. It allows Clients to verify the identity of the End-User based on the authentication performed by an Authorization Server, as well as to obtain basic profile information about the End-User in an interoperable and REST-like manner.

OpenID Connect allows clients of all types, including Web-based, mobile, and JavaScript clients, to request and receive information about authenticated sessions and end-users. The specification suite is extensible, allowing participants to use optional features such as encryption of identity data, discovery of OpenID Providers, and session management, when it makes sense for them.

{% hint style="info" %}
See [OpenID Connect specification documentation](http://openid.net/specs/openid-connect-core-1\_0.html) for more information.
{% endhint %}

### WS-Trust <a href="#ws-trust" id="ws-trust"></a>

The **Security Token Service** (STS) component of MWARE Identity Server enables you to configure the generic STS to issue claim-based security tokens. A claim-based security token is a common way for applications to acquire and authenticate the identity information they need about users inside their organization, in other organizations, and on the Internet. This Security Token Service is capable of issuing SAML 1.1 and SAML 2.0 tokens as recommended in WS-Trust and SAML Web Service Token Profile specifications.

The WSDL of this service can be accessed by clicking this URL: `https://(hostname):(port)/services/wso2carbon-sts?wsdl` . For instance, with the default configuration, the URL is `https://localhost:9443/services/wso2carbon-sts?wsdl` .

Both SAML 1.1 and SAML 2.0 token types are supported by default. The issued token type is decided based on the type of token defined in the RST (Request Security Token). Currently, the Bearer Subject Confirmation and Holder-Of-Key subject confirmation methods are both supported. With Holder-Of-Key, both Symmetric and Asymmetric key types are supported. You can obtain tokens containing claims that hold certain information about the subject. These claims can be extracted from the profiles or through custom claim callbacks which can be registered to the Carbon runtime.

{% hint style="info" %}
See [WS-Trust specification documentation](http://docs.oasis-open.org/ws-sx/ws-trust/v1.4/ws-trust.html) for more information
{% endhint %}

### Identity federation and a comparison with SSO <a href="#identity-federation-and-a-comparison-with-sso" id="identity-federation-and-a-comparison-with-sso"></a>

Identity and access management requirements are rapidly evolving over the years. Organizations cannot survive with authentication and authorization mechanisms that only span a single boundary of trust. Hence, these organizations often provide and consume services across trust boundaries, which may include partners, subsidiaries, customers or suppliers and may span across multiple buildings, cities, states, countries and even continents. Identity federation and Single Sign On (SSO) come into the picture to provide and consume these services across trust boundaries.

Identity federation and SSO have similarities as well as key differences. Identity federation enables users to access multiple applications using the same access credentials. This makes access easy, as users do not have to remember a different set of credentials for every application they use. However, the users have to provide their credentials to each one of the applications separately although the credentials used are the same. On the other hand, SSO enables users to provide their credentials once and obtain access to multiple applications. In SSO, the users are not prompted for their credentials when accessing each application until their session is terminated.
