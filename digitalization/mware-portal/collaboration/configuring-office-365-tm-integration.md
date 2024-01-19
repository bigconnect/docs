# Configuring Office 365™ Integration

Before you can use Office 365™ to create and edit Documents and Media files, you must configure MWARE Portal to connect with an application in the [Azure portal](https://portal.azure.com/).

**Note:** You must be an administrator to complete these steps.

### Register an Application with the Microsoft Identity Platform <a href="#register-an-application-with-the-microsoft-identity-platform" id="register-an-application-with-the-microsoft-identity-platform"></a>

First, configure your application with the Microsoft identity platform™. To do so, follow the steps described in [Microsoft’s documentation](https://docs.microsoft.com/en-gb/graph/auth-register-app-v2).

To construct a URL for the _Redirect URI_ parameter, follow this pattern:

```
https://[hostname]/o/document_library/onedrive/oauth2
```

Here’s the minimum permission set needed to use Office 365™ integration:

* Files.Read.All
* Files.ReadWrite.All

For more information about permissions, see [Microsoft’s documentation](https://docs.microsoft.com/graph/permissions-reference).

### Configuring the portal <a href="#configuring-liferay-dxp" id="configuring-liferay-dxp"></a>

Now you must connect your portal installation with your Microsoft identity platform™ application. You can do this at two scopes:

1. Globally, for all instances in your portal installation.
2. At the instance scope, for one or more instances in your portal installation.

You can override the global configuration for one or more instances by configuring those instances separately. Similarly, you can configure only the instances you want to connect to your application and leave the global configuration empty.

Follow these steps to configure your portal installation to connect to your application:

1.  Note that the configuration options are the same in the global and instance-level configurations.

    To access the global configuration, go to _Control Panel_ → _Configuration_ → _System Settings_ → _Documents and Media_.

    To access the instance-level configuration, go to _Control Panel_ → _Configuration_ → _Instance Settings_ → _Documents and Media_.
2. Under _VIRTUAL INSTANCE SCOPE_, select _OneDrive_.
3. Enter your application’s OAuth 2 client ID and client secret in the _Client ID_ and _Client Secret_ fields, respectively.
4. Enter your tenant ID in the _Tenant_ field. To find your tenant ID, see [Microsoft’s documentation](https://docs.microsoft.com/onedrive/find-your-office-365-tenant-id).
5. Click _Save_.

**Note:** Once enabled, you can disable this feature by deleting the client ID, client secret, and tenant values from the form.

![Figure 1: Enter your applications client ID, client secret, and tenant.](https://liferay-support.zendesk.com/hc/article\_attachments/360040158992/onedrive-system-settings.png)

