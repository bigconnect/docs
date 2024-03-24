# Using OpenAM

OpenAM is an open source single sign-on solution from the codebase of Sun’s System Access Manager product. You can use OpenAM to integrate the portal into an infrastructure that contains several different authentication schemes against different repositories of identities.

### Installation Notes <a href="#installation-notes" id="installation-notes"></a>

Follow the instructions in its documentation to install OpenAM. Note that OpenAM relies on cookie sharing between applications. Thus, in order for OpenAM to work, **all applications that require SSO must be in the same web domain**. You should also add the following property if you have enabled HTTPOnly cookies due to the way some web containers (like Apache Tomcat™) parse cookies that contain special characters:

```properties
com.iplanet.am.cookie.encode=true
```

You can install OpenAM on the same or different server as the portal. Be sure to review the context path and server hostname for your OpenAM server.

If you want to install OpenAM on the same server as the portal, you must deploy the OpenAM `.war`, downloadable from [here](https://backstage.forgerock.com/downloads/browse/am/archive/productId:openam). Otherwise, follow the instructions at the [OpenAM 13 site](https://backstage.forgerock.com/docs/openam/13/install-guide/) to install OpenAM.

NOTE

OpenAM 12 and below work with the portal, but are at end of life. Because of this, we recommend only OpenAM 13 for production use.

### Configuring OpenAM <a href="#configuring-openam" id="configuring-openam"></a>

Once you have it installed, you must do two things:

1. Create a portal administrative user in OpenAM
2. Enable OpenAM for authentication

Users are mapped back and forth by screen names, so be sure to match the OpenAM’s user ID to the screen name of your portal administrative user. For example, if the portal administrative user has a screen name of _admin_, register the user in OpenAM with the ID of _admin_ and the email address specified in the `admin.email.from.address` portal property. Once you have the user set up, log in to OpenAM using this user.

1. In the same browser window, log in to the portal as the administrative user (using the previous admin email address).
2.  Go to the Control Panel and click _Configuration_ → _Instance Settings_ → _Security_ → _SSO_. Then choose _OpenSSO_ in the list on the left.

    ![OpenSSO Configuration in the portal must be enabled for the integration to work.](https://resources.learn.liferay.com/images/dxp/latest/en/installation-and-upgrades/securing-liferay/configuring-sso/using-openam/images/01.png)
3. Modify the three URL fields (Login URL, Logout URL, and Service URL) so they point to your OpenAM server (in other words, only modify the host name portion of the URLs), check the _Enabled_ box, and click _Save_.

The portal then redirects users to OpenAM when they request the `/c/portal/login` URL—for example, when they click on the _Sign In_ link).

### Configuring OpenAM at a Different portal Scope <a href="#configuring-openam-at-a-different-liferay-scope" id="configuring-openam-at-a-different-liferay-scope"></a>

The portal’s OpenAM configuration can be applied at either the system scope or at the instance scope.

To configure the OpenAM SSO module at the system scope:

1. Navigate to the Control Panel
2. Click on _Configuration_ → _System Settings_ → _Security_ → _SSO_ → _OpenSSO_. You’ll see the settings below. The values configured here provide the default values for all portal instances. Enter them in the same format as you would when initializing a Java primitive type with a literal value.

| Property Label              | Property Key       | Description                                                                                                                                                                    | Type      |
| --------------------------- | ------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | --------- |
| **Version**                 | `version`          | OpenAM version to use (12 and below or 13)                                                                                                                                     | `String`  |
| **Enabled**                 | `enabled`          | Check this box to enable OpenAM authentication. Note that OpenAM works only if LDAP authentication is also enabled and the portal’s authentication type is set to screen name. | `boolean` |
| **Import from LDAP**        | `importFromLDAP`   | If this is checked, users authenticated from OpenAM that do not exist in the portal are imported from LDAP. LDAP must be enabled.                                              | `boolean` |
| **Login URL**               | `loginURL`         | The URL to the login page of the OpenAM server                                                                                                                                 | `String`  |
| **Logout URL**              | `logoutURL`        | The URL to the logout page of the OpenAM server                                                                                                                                | `String`  |
| **Service URL**             | `serviceURL`       | The URL by which OpenAM can be accessed to use the authenticated web services. If you are using OpenAM Express 8 or higher, you need to have the server running Java 6.        | `String`  |
| **Screen Name Attribute**   | `screenNameAttr`   | The name of the attribute on the OpenAM representing the user’s screen name                                                                                                    | `String`  |
| **Email Address Attribute** | `emailAddressAttr` | The name of the attribute on the OpenAM representing the user’s email address                                                                                                  | `String`  |
| **First Name Attribute**    | `firstNameAttr`    | The name of the attribute on the OpenAM representing the user’s first name                                                                                                     | `String`  |
| **Last Name Attribute**     | `lastNameAttr`     | The name of the attribute on the OpenAM representing the user’s last name                                                                                                      | `String`  |

To override these default settings for a particular portal instance, navigate to the Control Panel and click _Configuration_ → _Instance Settings_ → _Security_ → _SSO_. Then choose _OpenSSO_ in the list on the left.
