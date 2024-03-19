# Authentication Basics

By default, Liferay DXP uses the _Sign In_ widget to authenticate users.

NOTE

The Sign In widget appears on the default home page at `http[s]://[server-name:port]/web/guest/home`. If the Sign In widget is unavailable on any page, it can be accessed directly via its URL: `http[s]://[server-name:port]/c/portal/login`.

![The default Liferay home page](https://resources.learn.liferay.com/images/dxp/latest/en/installation-and-upgrades/securing-liferay/authentication-basics/images/01.png)

You can configure other methods of authenticating users and/or applications:

* [LDAP](https://learn.liferay.com/web/guest/w/dxp/users-and-permissions/connecting-to-a-user-directory/connecting-to-an-ldap-directory)
* [SAML](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/securing-liferay/configuring-sso/authenticating-with-saml/saml-authentication-process-overview)
* [Kerberos](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/securing-liferay/configuring-sso/authenticating-with-kerberos)
* [OpenID Connect](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/securing-liferay/configuring-sso/using-openid-connect)
* [Token-Based solutions](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/securing-liferay/configuring-sso/token-based-authentication)
* [OAuth 2.0](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/using-oauth2)

[Authentication Verifiers](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/securing-liferay/securing-web-services/using-authentication-verifiers) can manage authentication for remote applications, and [Authentication Pipelines](https://learn.liferay.com/web/guest/w/dxp/building-applications) define the ways users are validated by one or several systems.

### [Authentication Types](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/securing-liferay/authentication-basics#authentication-types) <a href="#authentication-types" id="authentication-types"></a>

Users can be configured to log in using one of three authentication types:

| Authentication Type | Description                                             | Used by Default? |
| ------------------- | ------------------------------------------------------- | ---------------- |
| Screen Name         | Determined by administrator or user at account creation | No               |
| Email Address       | Determined by administrator or user at account creation | Yes              |
| User ID             | Automatically generated when the account is created     | No               |

NOTE

Only one authentication type can be used at a time.

Regardless of the authentication type, users must always enter a password. You can create [Password Policies](https://learn.liferay.com/web/guest/w/dxp/users-and-permissions/roles-and-permissions/configuring-a-password-policy) to define password length, password format, expiration periods, and more.

_Authentication Type_ can be configured through the Control Panel or a properties file.

#### [Configuring Authentication Type Through the Control Panel](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/securing-liferay/authentication-basics#configuring-authentication-type-through-the-control-panel) <a href="#configuring-authentication-type-through-the-control-panel" id="configuring-authentication-type-through-the-control-panel"></a>

1. Navigate to the Control Panel
2. Click on _Configuration_ → _Instance Settings_ → _Platform_ → _User Authentication_
3.  Choose an option under the _How do users authenticate?_ selector.

    ![You can select from three types of authentication.](https://resources.learn.liferay.com/images/dxp/latest/en/installation-and-upgrades/securing-liferay/authentication-basics/images/03.png)

#### [Configuring Authentication Type Using Properties](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/securing-liferay/authentication-basics#configuring-authentication-type-using-properties) <a href="#configuring-authentication-type-using-properties" id="configuring-authentication-type-using-properties"></a>

To use the [`portal-ext.properties`](https://learn.liferay.com/dxp/latest/en/installation-and-upgrades/reference/portal-properties.html) file, paste in the below properties and uncomment the desired authentication type:

```properties
company.security.auth.type=emailAddress
#company.security.auth.type=screenName
#company.security.auth.type=userId
```

### [Using and Configuring the Sign In Widget](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/securing-liferay/authentication-basics#using-and-configuring-the-sign-in-widget) <a href="#using-and-configuring-the-sign-in-widget" id="using-and-configuring-the-sign-in-widget"></a>

The Sign In widget calls the various mechanisms (the portal database, an LDAP server, a SAML identity provider, or any of the ways users can authenticate) that authenticate users. Its behavior can be configured and customized in several ways.

#### [Disabling Guest Account Creation](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/securing-liferay/authentication-basics#disabling-guest-account-creation) <a href="#disabling-guest-account-creation" id="disabling-guest-account-creation"></a>

To prevent guest users from creating new user accounts:

1. Navigate to the Control Panel → _Configuration_ → _Instance Settings_ → _Platform_ → _User Authentication_.
2. Un-check _Allow strangers to create accounts?_.
3.  Click _Save_.

    ![Guests can't create accounts if this box is unchecked.](https://resources.learn.liferay.com/images/dxp/latest/en/installation-and-upgrades/securing-liferay/authentication-basics/images/04.png)

#### [Preventing Password Resets](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/securing-liferay/authentication-basics#preventing-password-resets) <a href="#preventing-password-resets" id="preventing-password-resets"></a>

If users should not be able to reset their own passwords, you can configure this from the same screen:

1. Navigate to the Control Panel → _Configuration_ → _Instance Settings_ → _Platform_ → _User Authentication_.
2. Uncheck _Allow users to request password reset links?_.
3. Click _Save_.

#### [Disabling the Password Section During Account Creation](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/securing-liferay/authentication-basics#disabling-the-password-section-during-account-creation) <a href="#disabling-the-password-section-during-account-creation" id="disabling-the-password-section-during-account-creation"></a>

Liferay DXP 2023.Q4+/Portal 7.4 GA92+

If the [_Allow Custom Password at Account Creation_](https://learn.liferay.com/web/guest/w/dxp/system-administration/configuring-liferay/virtual-instances/users#fields) field is unchecked, the password fields are unavailable at the time of account creation from the Sign In widget. This also applies to users invited by email for account creation.

#### [Configuring CAPTCHA or reCAPTCHA](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/securing-liferay/authentication-basics#configuring-captcha-or-recaptcha) <a href="#configuring-captcha-or-recaptcha" id="configuring-captcha-or-recaptcha"></a>

Prevent bots from creating and logging into accounts by enabling CAPTCHA or reCAPTCHA:

1.  Navigate to the Control Panel → _Configuration_ → _System Settings_ → _Security Tools_ → _CAPTCHA_.

    ![CAPTCHA is enabled by default.](https://resources.learn.liferay.com/images/dxp/latest/en/installation-and-upgrades/securing-liferay/authentication-basics/images/02.png)

    NOTE

    By default, _Create Account CAPTCHA_ and _Send Password CAPTCHA_ are enabled. If necessary, enable Message Boards CAPTCHA.
2. Choose a CAPTCHA engine. By default, Simple CAPTCHA is enabled. You can also select Google’s reCAPTCHA, which requires you configure the external service separately. If you select reCAPTCHA, supply your public and private key from Google.
3. The other options allow developers to customize the Simple CAPTCHA engine.
4. Click _Save_ when finished.

\
