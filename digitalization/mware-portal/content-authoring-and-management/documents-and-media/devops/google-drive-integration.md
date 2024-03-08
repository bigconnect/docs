# Google Drive Integration

Liferay provides a Marketplace plugin for integrating Documents and Media with Google Drive. When enabled, you can create shortcuts to your Google Drive files that enable you to view and manage them via Documents and Media. This plugin is available for both [Liferay Portal CE](https://web.liferay.com/marketplace/-/mp/application/105847499) and [Liferay DXP](https://web.liferay.com/marketplace/-/mp/application/98011653) systems. See [Installing Apps](https://learn.liferay.com/web/guest/w/dxp/system-administration/installing-and-managing-apps/installing-apps) for how to install the Liferay Plugin for Google Drive.

After installing the plugin, you must set up your Google project and configure Liferay’s Documents and Media application.

IMPORTANT

The Liferay Plugin for Google Drive is a Labs application available for Liferay CE Portal and Liferay DXP. Labs apps are experimental and not supported by Liferay. They’re released to accelerate the availability of useful and cutting-edge features. This status may change without notice. Use Labs apps at your own discretion.

### [Setting Up Your Google Project](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/documents-and-media/devops/google-drive-integration/enabling-links-to-google-drive-documents#setting-up-your-google-project) <a href="#setting-up-your-google-project" id="setting-up-your-google-project"></a>

Enabling the Liferay plugin requires an active Google Project with both the Google Drive API and Google Picker API enabled. The Google Drive API allows users to view and edit Google files (i.e., Docs, Sheets, and Slides) in their native editors. The Picker API allows users to view and select from digital assets (e.g., photos, videos, documents) stored in Google Drive when uploading files to Documents and Media. If you haven’t created a Project yet, see [Creating a Google Project](https://support.google.com/googleapi/answer/6251787?hl=en\&ref\_topic=7014522). Otherwise, see [Enabling APIs](https://support.google.com/googleapi/answer/6158841) to learn how to enable the Picker API for your project.

NOTE

If desired, you can enable the creation and editing of Google documents via Documents and Media apart from the Liferay Plugin for Google Drive. See [Enabling Document Creation and Editing with Google Drive](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/documents-and-media/devops/google-drive-integration/enabling-document-creation-and-editing-with-google-drive) for more information.

After enabling Google Picker API for your Project, [generate an API Key](https://support.google.com/googleapi/answer/6158862) for the Picker API, and [set up OAuth 2](https://support.google.com/cloud/answer/6158849). Setting up OAuth 2 includes configuring an OAuth consent screen, generating OAuth 2 credentials, and adding the Liferay request URI to the list of Authorized JavaScript Origins. When generating OAuth 2 credentials, ensure you’ve selected _Web application_ for the application type.

Once your Google Project is prepared, you must configure the Documents and Media application via the Liferay control panel. This requires the Picker API key, as well as the OAuth2 client ID and client secret.

### [Configuring Documents and Media](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/documents-and-media/devops/google-drive-integration/enabling-links-to-google-drive-documents#configuring-documents-and-media) <a href="#configuring-documents-and-media" id="configuring-documents-and-media"></a>

Follow these steps to configure the Documents and Media application.

1. Open the _Global Menu_ (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)), click on the _Control Panel_ tab, and go to _System Settings_ → _Documents and Media_.
2.  Click on _Google Drive_ under Virtual Instance Scope.

    ![Click on Google Drive under Virtual Instance Scope.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/devops/google-drive-integration/enabling-links-to-google-drive-documents/images/01.png)
3. Enter your project’s OAuth 2.0 _client ID_ and _client secret_.
4. Enter the _Picker API Key_.
5. Click on _Save_.

Once enabled, users can create Documents and Media shortcuts to Google assets.

NOTE

To disable this feature, click on the _Actions_ button for Google Drive and select _Reset Default Values_.

### [Creating Google Drive Shortcuts](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/documents-and-media/devops/google-drive-integration/enabling-links-to-google-drive-documents#creating-google-drive-shortcuts) <a href="#creating-google-drive-shortcuts" id="creating-google-drive-shortcuts"></a>

Follow these steps to create Documents and Media shortcuts for Google Drive files.

1. Open the Documents and Media application in a Site Asset Library.
2.  Click on the _Add_ button (![Add Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png)) in the Documents and Media tab and select _Google Drive Shortcut_.

    ![Click the Add button and select Google Drive Shortcut.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/devops/google-drive-integration/enabling-links-to-google-drive-documents/images/02.png)
3.  Click on _Select File_.

    ![Click on Select File.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/devops/google-drive-integration/enabling-links-to-google-drive-documents/images/03.png)
4. Select the desired Google account.
5.  Select the desired Google Drive file.

    ![Select the desired Google Drive file.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/devops/google-drive-integration/enabling-links-to-google-drive-documents/images/04.png)
6. Enter a _name_ and _description_ for the shortcut.
7. Click on _Publish_ when finished.

Once published, the Google Drive file can now be accessed and used in Liferay via the Documents and Media application. See [Linking to Files on Google Drive](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/documents-and-media/uploading-and-managing/linking-to-files-on-google-drive) for more information.
