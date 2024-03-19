# Email Settings

The email configuration is where you configure email notifications at the virtual instance scope.

To access these options,

1. Open the _Global Menu_ (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)) and navigate to _Control Panel_ → _Configuration_ → _Instance Settings_.
2. Select _Email_ under Platform.

Configuration options appear on the left:

| Configuration                                                                                                                                                                           | Purpose                                                                                            |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------- |
| [Account Created Notification](https://learn.liferay.com/web/guest/w/dxp/system-administration/configuring-liferay/virtual-instances/email-settings#account-created-notification)       | Enable/Disable email notifications for account creation and determine the notification’s template. |
| [Email Sender](https://learn.liferay.com/web/guest/w/dxp/system-administration/configuring-liferay/virtual-instances/email-settings#email-sender)                                       | Set the sender’s name and address for email templates.                                             |
| [Email Verification Notification](https://learn.liferay.com/web/guest/w/dxp/system-administration/configuring-liferay/virtual-instances/email-settings#email-verification-notification) | Determine the email template for requesting users to verify their email address.                   |
| [Mail Host Names](https://learn.liferay.com/web/guest/w/dxp/system-administration/configuring-liferay/virtual-instances/email-settings#mail-host-names)                                 | Enter mail hosts associated with the instance.                                                     |
| Mail Settings                                                                                                                                                                           | Connect a mail server to the instance.                                                             |
| [Password Changed Notification](https://learn.liferay.com/web/guest/w/dxp/system-administration/configuring-liferay/virtual-instances/email-settings#password-changed-notification)     | Determine the email template for password changes.                                                 |
| [Password Reset Notification](https://learn.liferay.com/web/guest/w/dxp/system-administration/configuring-liferay/virtual-instances/email-settings#password-reset-notification)         | Determine the email template for password reset requests.                                          |

NOTE

Your installation must have a [configured connection to a mail server](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/setting-up-liferay/configuring-mail) to send email notifications. Mail server configurations set at the instance scope override server level configurations.

For each notification template, you can view available template variables at the bottom of the page. Use these variables to populate emails dynamically.

### [Account Created Notification](https://learn.liferay.com/web/guest/w/dxp/system-administration/configuring-liferay/virtual-instances/email-settings#account-created-notification) <a href="#account-created-notification" id="account-created-notification"></a>

These templates define emails sent to users after creating an account. One template includes a password setup link, while the other does not.

Use the _Enabled_ checkbox at the top to enable this notification.

![Configure the account created email notification](https://resources.learn.liferay.com/images/dxp/latest/en/system-administration/configuring-liferay/virtual-instances/email-settings/images/01.png)

### [Email Sender](https://learn.liferay.com/web/guest/w/dxp/system-administration/configuring-liferay/virtual-instances/email-settings#email-sender) <a href="#email-sender" id="email-sender"></a>

The email sender specifies the virtual instance’s administrative name and email address for email notifications. You can include these values in notification templates using the `[$FROM_NAME$]` and `[$FROM_ADDRESS$]` variables. Defaults from [portal properties](https://learn.liferay.com/reference/latest/en/dxp/propertiesdoc/portal.properties.html#Admin%20Portlet) are `admin.email.from.name` and `admin.email.from.address`. This name and email address appear in the _From_ field in all email messages sent by the virtual instance.

![Configure the from name and address for system emails.](https://resources.learn.liferay.com/images/dxp/latest/en/system-administration/configuring-liferay/virtual-instances/email-settings/images/02.png)

### [Email Verification Notification](https://learn.liferay.com/web/guest/w/dxp/system-administration/configuring-liferay/virtual-instances/email-settings#email-verification-notification) <a href="#email-verification-notification" id="email-verification-notification"></a>

This template defines emails sent to users when asked to verify their email address.

![Verify the email address of users.](https://resources.learn.liferay.com/images/dxp/latest/en/system-administration/configuring-liferay/virtual-instances/email-settings/images/03.png)

### [Mail Host Names](https://learn.liferay.com/web/guest/w/dxp/system-administration/configuring-liferay/virtual-instances/email-settings#mail-host-names) <a href="#mail-host-names" id="mail-host-names"></a>

You can specify additional mail host names here that are owned by your organization for the virtual instance. Enter one mail host name per line, but don’t enter the one specified in the virtual instance configuration.

![Configure additional mail host names for the virtual instance.](https://resources.learn.liferay.com/images/dxp/latest/en/system-administration/configuring-liferay/virtual-instances/email-settings/images/04.png)

### [Password Changed Notification](https://learn.liferay.com/web/guest/w/dxp/system-administration/configuring-liferay/virtual-instances/email-settings#password-changed-notification) <a href="#password-changed-notification" id="password-changed-notification"></a>

This template defines emails notifying users that their password has been changed.

![Notify users when their password changes.](https://resources.learn.liferay.com/images/dxp/latest/en/system-administration/configuring-liferay/virtual-instances/email-settings/images/05.png)

### [Password Reset Notification](https://learn.liferay.com/web/guest/w/dxp/system-administration/configuring-liferay/virtual-instances/email-settings#password-reset-notification) <a href="#password-reset-notification" id="password-reset-notification"></a>

This template is for password reset requests.

![Notify users when a password reset has been made.](https://resources.learn.liferay.com/images/dxp/latest/en/system-administration/configuring-liferay/virtual-instances/email-settings/images/06.png)
