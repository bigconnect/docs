# Configuring Google Docs™ Integration

Before you can use Google Docs™ to create and edit Documents and Media files, you must configure the portal to connect with an application in the [Google API Console](https://console.developers.google.com/).

**Note:** You must be an administrator to complete these steps.

### Configure Your Google Project <a href="#configure-your-google-project" id="configure-your-google-project"></a>

First, you must configure your Google project to use the Google Drive™ API and set up OAuth 2 for use with that project.

1. Go to the [Google API Console](https://console.developers.google.com/). If you don’t have a suitable project, [create a new one](https://support.google.com/googleapi/answer/6251787?hl=en\&ref\_topic=7014522).
2. Enable the Google Drive™ API for your project. For instructions, see the Google API Console documentation on [enabling and disabling APIs](https://support.google.com/googleapi/answer/6158841).
3. Create an OAuth 2 client ID for your Google project. For instructions, see the Google API Console documentation on [setting up OAuth 2.0](https://support.google.com/googleapi/answer/6158849). Select _Web application_ when prompted to select your application type. Take note of the client ID and client secret that appear—you’ll need them to configure the portal to use the Google Drive™ API.

### Configuring the Portal <a href="#configuring-the-portal" id="configuring-the-portal"></a>

Now that you have a Google project set up for use with the portal, you must connect your installation to that project. You can do this at two scopes:

1. Globally, for all instances in your portal installation.
2. At the instance scope, for one or more instances in your portal installation.

You can override the global configuration for one or more instances by configuring those instances separately. Similarly, you can configure only the instances you want to connect to your Google project and leave the global configuration empty.

Follow these steps to configure your portal installation to connect to your Google project:

1.  Note that the configuration options are the same in the global and instance-level configurations.

    To access the global configuration, go to _Control Panel_ → _Configuration_ → _System Settings_ → _Documents and Media_.

    To access the instance-level configuration, go to _Control Panel_ → _Configuration_ → _Instance Settings_ → _Documents and Media_.
2. Under _VIRTUAL INSTANCE SCOPE_, select _Google Drive_.
3. Enter your Google project’s OAuth 2 client ID and client secret into the _Client ID_ and _Client Secret_ fields.
4. Click _Save_.

**Note:** To turn this feature off, delete the client ID and client secret values from the form.

**Note:** You can ignore the _Picker API Key_ field. This field is unrelated to the Google Docs™ online editing features in the portal.

![Figure 1: Enter your Google projects OAuth 2 client ID and client secret.](https://liferay-support.zendesk.com/hc/article\_attachments/360029909211/google-drive-system-settings.png)

