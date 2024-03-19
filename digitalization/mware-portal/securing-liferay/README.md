# Securing Liferay

Liferay DXP is built with security in mind. This includes mitigation of common security vulnerabilities and exploits like those described by the [OWASP Top 10](https://www.owasp.org/index.php/Top\_10\_2013-Top\_10) and the [CWE/SANS Top 25](https://www.sans.org/top25-software-errors/).

There are several aspects of securing a Liferay installation—including, but not limited to, following the best security practices for your hosting environment, database, search provider, application server, and Liferay DXP itself.

NOTE

For example, Liferay DXP relies on the application server for sanitizing CRLF in HTTP headers. You must ensure this is configured properly on the application server. Skipping this configuration may result in security verification products, like Veracode, flagging false positives in security reports.

Here you’ll learn the basic elements to secure Liferay DXP. This includes configuring how users authenticate to your Liferay DXP installation, authorizing users with permissions, configuring secure access to Liferay DXP Web Services, and fine-tuning security features on an as-needed basis.

IMPORTANT

Customers are advised to deploy security patches as they are released. For community and CE installations, we recommend always using the latest community release, which contains all previous security patches.

### [Authentication](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/securing-liferay#authentication) <a href="#authentication" id="authentication"></a>

Liferay DXP authentication is flexible. By default, users log into Liferay DXP by using the _Sign In_ widget, which uses the database to authenticate the user. By default, guests can use the Sign In widget to create accounts with default permissions. Nearly every element of the default authentication experience can be changed by an administrator. For example,

* You can configure [Multi-Factor authentication](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/securing-liferay/multi-factor-authentication/using-multi-factor-authentication).
* You can [use an SSO to manage](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/securing-liferay/configuring-sso) authentication.
* Liferay can also be [integrated with LDAP](https://learn.liferay.com/web/guest/w/dxp/users-and-permissions/connecting-to-a-user-directory/connecting-to-an-ldap-directory) to validate users instead of using the portal database.
* Guest account creation can be [turned off](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/securing-liferay/authentication-basics#disabling-guest-account-creation).

To learn more, see [Authentication Basics](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/securing-liferay/authentication-basics).

### [Permissions](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/securing-liferay#permissions) <a href="#permissions" id="permissions"></a>

Liferay DXP has a robust role-based access control (RBAC) system. Users can be assigned to Sites, Teams, User Groups, or Organizations. Custom Roles can be created, permissions can be assigned to these Roles, and those Roles can be assigned to Users. Roles are scoped to apply only in a specific context, such as a Site, Organization, or globally. See [Roles and Permissions](https://learn.liferay.com/web/guest/w/dxp/users-and-permissions/roles-and-permissions/understanding-roles-and-permissions) for more information.

### [Securing Web Services](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/securing-liferay#securing-web-services) <a href="#securing-web-services" id="securing-web-services"></a>

Liferay Web Services have a multi-layered and configurable approach to security and authorization:

* [Service Access Policies](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/securing-liferay/securing-web-services/setting-service-access-policies) control access to remote APIs.
* [Authentication Verifiers](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/securing-liferay/securing-web-services/using-authentication-verifiers) verify provided credentials.
* [Cross-Origin Resource Sharing](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/securing-liferay/securing-web-services/setting-up-cors) configuration can enable retrieving resources from trusted sources only.

See [Introduction to Securing Web Services](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/securing-liferay/securing-web-services) to learn more.

### [Fine-Tuning Security](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/securing-liferay#fine-tuning-security) <a href="#fine-tuning-security" id="fine-tuning-security"></a>

There are many ways to fine-tune or disable additional security features:

* Configure Liferay Portal’s HTTPS [web server](https://learn.liferay.com/reference/latest/en/dxp/propertiesdoc/portal.properties.html#Web%20Server) address.
* Configure the list of allowed servers to which users can be [redirected](https://learn.liferay.com/reference/latest/en/dxp/propertiesdoc/portal.properties.html#Redirect).
* Configure the list of [portlets](https://learn.liferay.com/reference/latest/en/dxp/propertiesdoc/portal.properties.html#Portlet) that can be accessed from any page.
* Configure the file types allowed to be uploaded and downloaded.

These features can be configured using [portal properties](https://learn.liferay.com/reference/latest/en/dxp/propertiesdoc/portal.properties.html).

WARNING

Liferay Portal’s philosophy is “secure by default.” Please exercise significant caution when modifying security-specific defaults or white-lists. Such actions may lead to security misconfiguration and an insecure deployment.

\
