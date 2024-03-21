# Using Third Parties for Translation

MWARE Portal can integrate with the following service APIs to generate automatic translations of Content Pages and Web Content.

* Google Cloud Translation
* Amazon Translate
* Microsoft Translator

To use this feature, you must first set up the translation service you are using and then enable it in system settings. Once enabled, you can use the service to generate translations for Content Pages and Web Content.

IMPORTANT

Only one service can be active at a time. If you enable multiple services, the system uses the first one available.

### Enabling Google Cloud Translation <a href="#enabling-google-cloud-translation" id="enabling-google-cloud-translation"></a>

Enabling Google Cloud Translation requires an active account and a Google Cloud project with Cloud Translation API enabled. This project must also have a service account with a JSON account key. See official Google Cloud Translation documentation for more information.

When ready, follow these steps:

1.  Open the _Global Menu_(![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)), click the _Control Panel_ tab, and go to _System Settings_ → _Translation_ → _Translator Using Google Cloud_.

    ![Go to Translator Using Google Cloud.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/translating-pages-and-content/using-third-parties-for-translation/images/01.png)
2. Check _Enabled_.
3. Enter a valid Google Cloud service account key in JSON format.
4. Click _Save_.

### Enabling Amazon Translate <a href="#enabling-amazon-translate" id="enabling-amazon-translate"></a>

Available 7.4 +

Enabling Amazon Translate requires an active Amazon AWS account. See official Amazon Translate documentation for more information.

When ready, follow these steps:

1.  Open the _Global Menu_(![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)), click the _Control Panel_ tab, and go to _System Settings_ → _Translation_ → _Translator Using AWS_.

    ![Go to Translator Using AWS.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/translating-pages-and-content/using-third-parties-for-translation/images/02.png)
2. Check _Enabled_.
3. Enter a valid _Access Key_ and _Secret Key_.
4. Enter your region. The default value is `us-west-1`.
5. Click _Save_.

### Enabling Microsoft Translator <a href="#enabling-microsoft-translator" id="enabling-microsoft-translator"></a>

Available 7.4 +

Enabling Microsoft Translator requires an active Microsoft Azure account. See official Microsoft Translator documentation for more information.

When ready, follow these steps:

1.  Open the _Global Menu_(![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)), click the _Control Panel_ tab, and go to _System Settings_ → _Translation_ → _Translator Using Azure_.

    ![Go to Translator Using Azure.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/translating-pages-and-content/using-third-parties-for-translation/images/03.png)
2. Check _Enabled_.
3. Enter a valid _Subscription Key_ and _Resource Location_ (region).
4. Click _Save_.

### Using Automatic Translation <a href="#using-automatic-translation" id="using-automatic-translation"></a>

Once a translation service is enabled, auto translate buttons are added to the translation interface for Content Pages and Web Content.

Follow these steps to automatically translate fields:

1. Open the _Pages_ or _Web Content_ application.
2.  Click the _Actions_ button (![Actions Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) for the content or Page you want to translate and select _Translate_.

    ![Open the Web Content translation interface.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/translating-pages-and-content/using-third-parties-for-translation/images/04.png)
3.  Use the language flags to select the _languages_ you want to translate.

    NOTE

    The languages available for translation are determined by your instance’s localization settings. See Initial Instance Localization and Virtual Instance Localization for more information.
4.  Click the _Auto Translate_ button to translate all Web Content fields into the selected language.

    ![Click the top left Auto Translate button to translate all Web Content fields.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/translating-pages-and-content/using-third-parties-for-translation/images/05.png)

    Alternatively, click a field’s individual _Auto Translate_ button (![Auto Translate Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-translate.png)) to only translate its text.

    ![Click a field's individual Auto Translate button to only translate its text.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/translating-pages-and-content/using-third-parties-for-translation/images/06.png)
5. Click _Save as Draft_ or _Publish_ when finished.

\
