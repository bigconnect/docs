# Email Instance Settings

The Email configuration is your one-stop-shop for all your email notification needs. To access the Email configuration settings for your instance, open the Control Panel and navigate to _Configuration_ → _Instance Settings_ and select the _Email_ category under the PLATFORM section. The Email configuration contains six entries:

* Account Created Notification
* Email Sender
* Email Verification Notification
* Mail Host Names
* Password Changed Notification
* Password Reset Notification

Each configuration entry is described in the corresponding section below.

### Account Created Notification <a href="#account-created-notification" id="account-created-notification"></a>

The Account Created Notification entry defines the email templates, with and without the new User’s password included in the body of the text, for the message sent to notify Users when they create a new account. You can specify whether this email is sent by checking/unchecking the _Enabled_ checkbox at the top. Use the template variables listed at the bottom of the configuration under the “Definition of Terms” heading to help build your email template.

### Email Sender <a href="#email-sender" id="email-sender"></a>

The Email Sender entry specifies the virtual instance’s administrative Name and Address for email notifications, declared as the `[$FROM_NAME$]` and `[$FROM_ADDRESS$]` variables respectively in the email templates. By default, they are from the `admin.email.from.name` and `admin.email.from.address` portal properties). This name and email address appear in the _From_ field in all email messages sent by the virtual instance.

![Figure 1: Customize the email template for the email messages sent to new Users.](https://liferay-support.zendesk.com/hc/article\_attachments/360035538551/instance-settings-account-created.png)

### Email Verification Notification <a href="#email-verification-notification" id="email-verification-notification"></a>

The Email Verification Notification entry defines the email template for the message sent to Users when asked to verify their Email Address. Use the template variables listed at the bottom of the configuration under the “Definition of Terms” heading to help build your email template.

### Mail Host Names <a href="#mail-host-names" id="mail-host-names"></a>

The Mail Host Names entry specifies which mail host names are owned by your organization for the virtual instance. Enter one mail host name per line, besides the one specified in the General tab.

### Password Changed Notification <a href="#password-changed-notification" id="password-changed-notification"></a>

The Password Changed Notification entry defines the email template for the message sent to notify Users when their password has changed. Use the template variables listed at the bottom of the configuration under the “Definition of Terms” heading to help build your email template.

### Password Reset Notification <a href="#password-reset-notification" id="password-reset-notification"></a>

The Password Reset Notification entry defines the email template for the message sent to notify Users when a request has been made to reset their password. Use the template variables listed at the bottom of the configuration under the “Definition of Terms” heading to help build your email template.

![Figure 2: There are some handy variables available for use in email templates.](https://liferay-support.zendesk.com/hc/article\_attachments/360035538571/instance-settings-definition-of-terms.png)

