# Enabling Document Creation and Editing with Microsoft Office 365

Before you can use Office 365™ to create and edit Documents and Media files, you must configure the portal to connect with an application in the Azure portal. You must have administrator rights to perform the following steps.

### Register an Application with the Microsoft Identity Platform <a href="#register-an-application-with-the-microsoft-identity-platform" id="register-an-application-with-the-microsoft-identity-platform"></a>

First, configure your application with the Microsoft identity platform™. To do so, follow the steps described in Microsoft’s documentation.

To construct a URL for the _Redirect URI_ parameter, follow this pattern: `https://[hostname]/o/document_library/onedrive/oauth2`

Here’s the minimum permission set needed to use Office 365™ integration:

* `Files.Read.All`
* `Files.ReadWrite.All`

For more information about permissions, see Microsoft’s documentation.

### Configuring the portal <a href="#configuring-dxp" id="configuring-dxp"></a>

Now you must connect your portal installation with your Microsoft identity platform™ application. You can do this at two scopes:

* Globally, for all instances in your portalinstallation.
* At the instance scope, for one or more instances in your portal installation.

You can override the global configuration for one or more instances by configuring those instances separately. Similarly, you can configure only the instances you want to connect to your application and leave the global configuration empty.

#### Global Level Configuration <a href="#global-level-configuration" id="global-level-configuration"></a>

1. Navigate to _Control Panel_ → _System Settings_ → _Documents and Media_.
2.  Click _OneDrive_ in the left menu.

    ![Enter your Client ID, Client Secret, and Tenant.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/devops/enabling-document-creation-and-editing-with-microsoft-office-365/images/01.png)
3. Enter your application’s OAuth 2 client ID and client secret in the _Client ID_ and _Client Secret_ fields, respectively.
4. Enter your tenant ID in the _Tenant_ field. To find your tenant ID, see Microsoft’s documentation.
5. Click _Save_.

Access has been granted at the global level.

#### Instance Level Configuration <a href="#instance-level-configuration" id="instance-level-configuration"></a>

1. Navigate to the _Control Panel_ → _Instance Settings_ → \*Content and Data → _Documents and Media_.
2.  Click _OneDrive_ in the left navigation menu.

    ![Enter your credentials in the corresponding fields at the instance level.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/devops/enabling-document-creation-and-editing-with-microsoft-office-365/images/02.png)
3. Enter your application’s OAuth 2 client ID and client secret in the _Client ID_ and _Client Secret_ fields, respectively.
4. Enter your tenant ID in the _Tenant_ field. To find your tenant ID, see Microsoft’s documentation.
5. Click _Save_.

**Note:** Once enabled, you can disable this feature by deleting the client ID, client secret, and tenant values from the form.

\
