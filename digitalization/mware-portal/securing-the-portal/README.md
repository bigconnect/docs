# Securing the Portal

MWARE Portal is built with security in mind. This includes mitigation of common security vulnerabilities and exploits like those described by the [OWASP Top 10](https://www.owasp.org/index.php/Top\_10\_2013-Top\_10) and the [CWE/SANS Top 25](https://www.sans.org/top25-software-errors/).

There are several aspects of securing a portal installation—including, but not limited to, following the best security practices for your hosting environment, database, search provider, application server, and the portal itself.

{% hint style="info" %}
For example, the portal relies on the application server for sanitizing CRLF in HTTP headers. You must ensure this is configured properly on the application server. Skipping this configuration may result in security verification products, like Veracode, flagging false positives in security reports.
{% endhint %}

Here you’ll learn the basic elements to secure the portal. This includes configuring how users authenticate to your portal installation, authorizing users with permissions, configuring secure access to portal Web Services, and fine-tuning security features on an as-needed basis.

IMPORTANT

Customers are advised to deploy security patches as they are released. For community and CE installations, we recommend always using the latest community release, which contains all previous security patches.

### [Authentication](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/securing-liferay#authentication) <a href="#authentication" id="authentication"></a>

MWARE Portal authentication is flexible. By default, users log into the portal by using the _Sign In_ widget, which uses the database to authenticate the user. By default, guests can use the Sign In widget to create accounts with default permissions. Nearly every element of the default authentication experience can be changed by an administrator. For example,

* You can configure [Multi-Factor authentication](https://app.gitbook.com/o/-LvB5aUmu6FceBG17Ndx/s/-Lv6YU\_27lR-b2ZLvWS\_/\~/changes/893/digitalization/mware-portal/securing-the-portal/multi-factor-authentication).
* You can [use SSO](https://app.gitbook.com/o/-LvB5aUmu6FceBG17Ndx/s/-Lv6YU\_27lR-b2ZLvWS\_/\~/changes/893/digitalization/mware-portal/securing-the-portal/configuring-single-sign-on) to manage authentication.
* The portal can also be [integrated with LDAP](../users-and-permissions/connecting-to-a-user-directory.md) to validate users instead of using the portal database.
* Guest account creation can be turned off.

To learn more, see [Authentication Basics](https://app.gitbook.com/o/-LvB5aUmu6FceBG17Ndx/s/-Lv6YU\_27lR-b2ZLvWS\_/\~/changes/893/digitalization/mware-portal/securing-the-portal/authentication-basics).

### Permissions <a href="#permissions" id="permissions"></a>

The portal has a robust role-based access control (RBAC) system. Users can be assigned to Sites, Teams, User Groups, or Organizations. Custom Roles can be created, permissions can be assigned to these Roles, and those Roles can be assigned to Users. Roles are scoped to apply only in a specific context, such as a Site, Organization, or globally. See [Roles and Permissions](../users-and-permissions/roles-and-permissions/) for more information.

### Securing Web Services <a href="#securing-web-services" id="securing-web-services"></a>

Portal Web Services have a multi-layered and configurable approach to security and authorization:

* [Service Access Policies](https://app.gitbook.com/o/-LvB5aUmu6FceBG17Ndx/s/-Lv6YU\_27lR-b2ZLvWS\_/\~/changes/893/digitalization/mware-portal/securing-the-portal/securing-web-services/setting-service-access-policies) control access to remote APIs.
* [Authentication Verifiers](https://app.gitbook.com/o/-LvB5aUmu6FceBG17Ndx/s/-Lv6YU\_27lR-b2ZLvWS\_/\~/changes/893/digitalization/mware-portal/securing-the-portal/securing-web-services/using-authentication-verifiers) verify provided credentials.
* [Cross-Origin Resource Sharing](https://app.gitbook.com/o/-LvB5aUmu6FceBG17Ndx/s/-Lv6YU\_27lR-b2ZLvWS\_/\~/changes/893/digitalization/mware-portal/securing-the-portal/securing-web-services/setting-up-cors) configuration can enable retrieving resources from trusted sources only.

See [Introduction to Securing Web Services](https://app.gitbook.com/o/-LvB5aUmu6FceBG17Ndx/s/-Lv6YU\_27lR-b2ZLvWS\_/\~/changes/893/digitalization/mware-portal/securing-the-portal/securing-web-services) to learn more.

### Fine-Tuning Security <a href="#fine-tuning-security" id="fine-tuning-security"></a>

There are many ways to fine-tune or disable additional security features:

* Configure the Portal’s HTTPS web server address.
* Configure the list of allowed servers to which users can be redirected.
* Configure the list of portlets that can be accessed from any page.
* Configure the file types allowed to be uploaded and downloaded.

These features can be configured using portal properties.

{% hint style="warning" %}
The Portal’s philosophy is “secure by default.” Please exercise significant caution when modifying security-specific defaults or white-lists. Such actions may lead to security misconfiguration and an insecure deployment.
{% endhint %}

\
