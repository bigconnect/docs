# Linking to Google Drive™

You can create Document Library files that link to files in Google Drive™ and Google Photos™. This lets you access your Google files from the Document Library. Note that this functionality isn’t available by default. To enable it, you must complete these steps:

1. Create and/or configure a Google project capable of communicating with your portal instance. The [Google Picker API](https://developers.google.com/picker/) must be enabled for this project. This API lets you select Google files to link to. You must also create the credentials the Google project needs to communicate with your portal instance.
2. Configure your portal to communicate with your Google project.

This article shows you how to complete these steps and finishes with an example of linking to a Google file from the Document Library.

**Note:** You can also use Google Docs™ for online file creation and editing.

### Configure Your Google Project <a href="#configure-your-google-project" id="configure-your-google-project"></a>

Follow these steps to create and/or configure your Google project so it can communicate with your portal instance:

1. Go to the [Google API Console](https://console.developers.google.com/). If you don’t have a suitable project, [create a new one](https://support.google.com/googleapi/answer/6251787?hl=en\&ref\_topic=7014522).
2. Enable the Google Picker API for your project. For instructions, see the Google API Console documentation on [enabling and disabling APIs](https://support.google.com/googleapi/answer/6158841).
3. Create an OAuth 2 client ID in your Google project. For instructions, see the Google API Console documentation on [setting up OAuth 2.0](https://support.google.com/googleapi/answer/6158849). Enter these values when creating your client ID:
   * **Application type:** Web application
   * **Name:** Google Docs Hook
   * **Authorized JavaScript origins**: `[portal-instance-URL]` (e.g., `http://localhost:8080` is the default for local development machines)
   * **Authorized redirect URIs**: `[portal-instance-URL]/oath2callback`
4. Create a new API key in your Google project. For instructions, see the Google API Console documentation on [creating API keys](https://support.google.com/googleapi/answer/6158862?hl=en). Be sure to restrict the key to HTTP referrers (web sites), and set it to accept requests from your portal instance’s URL.

Your new OAuth client ID and public API access key now appear on your Google project’s Credentials screen. Keep this screen open to reference these values as you specify them in the portal.

### Configure Your Portal <a href="#configure-your-portal" id="configure-your-portal"></a>

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
4. In the field _Picker API Key_, enter the API key you created in the previous section.
5. Click _Save_.

![Figure 1: Enter your Google projects OAuth 2 client ID, OAuth 2 client secret, and Picker API key.](https://liferay-support.zendesk.com/hc/article\_attachments/360039599252/google-drive-system-settings.png)

### Creating Linked Files <a href="#creating-linked-files" id="creating-linked-files"></a>

With the preceding configuration steps complete, you can create files in your Document Library that link to files in Google Drive™ or images in Google Photos™. Follow these steps to do so:

1.  In your Document Library, click the _Add_ button (![Add](https://liferay-support.zendesk.com/hc/article\_attachments/360039599192/icon-add.png)) and select _Google Drive Shortcut_. The _New Google Drive Shortcut_ screen appears.

    <img src="https://liferay-support.zendesk.com/hc/article_attachments/360039599232/dm-new-google-drive-shortcut.png" alt="Figure 2: Select New Google Drive Shortcut from the Add menu in your Document Library." data-size="original">

    Figure 2: Select \*New Google Drive Shortcut\* from the \*Add\* menu in your Document Library.
2. Click the _Select File_ button to open Google’s file picker.
3. Use the file picker to select a file from Google Drive™ or Google Photos™.
4. Click _Publish_.

![Figure 3: You can select files from Google Drive™ or your photos.](https://liferay-support.zendesk.com/hc/article\_attachments/360039599272/dm-google-select-a-file.png)

A new file entry appears for the Google document you linked to. You can view the file entry as you would any file entry. The Google document’s contents show in the file entry’s preview pane. As with any file entry, the _Options_ button (![Options](https://liferay-support.zendesk.com/hc/article\_attachments/360039599212/icon-options.png)) gives you access to the Download, Edit, Move, Permissions, Move to Recycle Bin, and Checkin/Checkout/Cancel Checkout options.
