# System for Cross-domain Identity Management (SCIM)

System for Cross-domain Identity Management or SCIM, is an open standard that automates user provisioning. In other words, it’s a standard way to create, update, and deactivate user identities. SCIM provides a unified, RFC-compliant way to keep user/group data in sync between different applications. It consists of

* A defined schema for representing users and groups. See [User Resource Schema](https://datatracker.ietf.org/doc/html/rfc7643#section-4.1) and [Group Resource Schema](https://datatracker.ietf.org/doc/html/rfc7643#section-4.2) for more information.
* A RESTful API to run CRUD operations on those users and groups to keep them in sync. See [SCIM User API Basics](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/securing-liferay/developer-guide/scim-user-api-basics) and [SCIM Group API Basics](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/securing-liferay/developer-guide/scim-group-api-basics) for more information.

IMPORTANT

This feature is currently behind a [beta feature flag](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/system-administration/configuring-liferay/feature-flags#beta-feature-flags).

Using SCIM, you can automate the exchange of user identity information between your company’s applications and service providers like Liferay securely. This is useful if your company uses multiple applications and wants to keep user data in sync without custom implementations.

### [Registering a SCIM Client](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/securing-liferay/system-for-cross-domain-identity-management-scim#registering-a-scim-client) <a href="#registering-a-scim-client" id="registering-a-scim-client"></a>

1. Open the _Global Menu_ (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)) and navigate to _Control Panel_ → _Instance Settings_ → _Security_ → _SCIM_.
2.  Enter a name in the OAuth 2 Application Name field.

    The name entered here is used to generate the SCIM client ID. This ID links users and groups with the SCIM client. If your SCIM client name is `Test SCIM Client`, the generated SCIM client ID is `SCIM_test-scim-client`.
3.  Set the Matcher Field to `userName` or `email`.

    The SCIM client uses this field to match the user data in the service provider and the connected application. This avoids issues with provisioning and prevents data duplication.

    ![Enter a name and set the matcher field for the new SCIM client.](https://resources.learn.liferay.com/images/dxp/latest/en/installation-and-upgrades/securing-liferay/system-for-cross-domain-identity-management-scim/images/01.png)
4.  The Access Token field is empty at first. Click _Generate_ and click _OK_ to populate the field with an access token. An access token expires after 1 year unless revoked. If this is not the first time you’re generating an access token, it overwrites the existing token, but it remains valid for 10 more days. You can use this grace period to configure the new token in the client application.

    You can invoke the SCIM APIs by setting the access token in the `Authorization` request header. To revoke all access tokens, click _Revoke_ and click _OK_.
5. Click _Save_.

NOTE

A scheduler runs daily to retrieve all OAuth 2 applications that start with the `SCIM_` prefix. If the access token’s expiry is 30 days, 7 days, or 1 day, all administrators are notified in Liferay and by email.

### [Linking a User to a SCIM Client](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/securing-liferay/system-for-cross-domain-identity-management-scim#linking-a-user-to-a-scim-client) <a href="#linking-a-user-to-a-scim-client" id="linking-a-user-to-a-scim-client"></a>

1. Open the _Global Menu_ (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)) and navigate to _Control Panel_ → _Users and Organizations_.
2. Select a user from the Users tab.
3.  Scroll down to the CUSTOM FIELDS section and enter the SCIM client ID in the _Scimclientid_ field.

    ![Enter the SCIM client ID to link the user to the SCIM client.](https://resources.learn.liferay.com/images/dxp/latest/en/installation-and-upgrades/securing-liferay/system-for-cross-domain-identity-management-scim/images/02.png)
4. Click _Save_.

### [Linking a User Group to a SCIM Client](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/securing-liferay/system-for-cross-domain-identity-management-scim#linking-a-user-group-to-a-scim-client) <a href="#linking-a-user-group-to-a-scim-client" id="linking-a-user-group-to-a-scim-client"></a>

1. Open the _Global Menu_ (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)) and navigate to _Control Panel_ → _Users Groups_.
2. Select a user group, click _Actions_ (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) and select _Edit_.
3.  Enter the SCIM client ID in the _Scimclientid_ field.

    ![Enter the SCIM client ID to link the user group to the SCIM client.](https://resources.learn.liferay.com/images/dxp/latest/en/installation-and-upgrades/securing-liferay/system-for-cross-domain-identity-management-scim/images/03.png)
4. Click _Save_.

\
