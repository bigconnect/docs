# Translating Web Content

Liferay provides integrated tools for translating web content articles, so you can create engaging, localized content for your global users. With these tools, you can manually translate web content or import translations as XLIFF files.

If desired, you can [enable a custom workflow](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/translating-pages-and-content/enabling-workflows-for-translations) to direct the review and publishing process for translations.

NOTE

The languages available for translation are determined by your instance’s localization settings. See [Initial Instance Localization](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/setting-up-liferay/initial-instance-localization) and [Virtual Instance Localization](https://learn.liferay.com/web/guest/w/dxp/system-administration/configuring-liferay/virtual-instances/localization) for more information.

### [Manually Translating Content](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/translating-pages-and-content/translating-web-content#manually-translating-content) <a href="#manually-translating-content" id="manually-translating-content"></a>

NOTE

To manually translate web content, users must have either View and Update permissions for the asset or Translation permissions for one or more languages. See [Managing Permissions for Translation](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/translating-pages-and-content/managing-translation-permissions) for more information.

1. Open the _Site Menu_ (![Site Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu.png)) and navigate to _Content & Data_ → _Web Content_.
2.  Click _Actions_ (![Actions icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) for the content you want to translate and select _Translate_.

    This redirects you to the translation interface, where you can view the content’s original text alongside your translation.

    The left column displays the language you’re translating from, and the right column provides editable fields you can use for your translation.

    ![Selecting translate redirects you to the translation interface.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/translating-pages-and-content/translating-web-content/images/01.png)
3.  Select the language you want to translate.

    IMPORTANT

    Users with content Update permissions can translate the original text into any language.

    Users with `translate` permissions can only translate the origin text into languages for which they have permission.

    ![Select the language you want to translate.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/translating-pages-and-content/translating-web-content/images/02.png)
4.  Enter your translation for the available fields.

    NOTE

    The friendly URL for translations remains empty unless filled in by the user. If the friendly URL is left empty, the default language’s friendly URL is used.
5.  Click _Publish_ to create a new version of the web content or initiate a workflow, if it’s enabled.

    Alternatively, click _Save as Draft_ to save and publish your translation later.

### [Publishing Translated Web Content Using Localizable Fields](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/translating-pages-and-content/translating-web-content#publishing-translated-web-content-using-localizable-fields) <a href="#publishing-translated-web-content-using-localizable-fields" id="publishing-translated-web-content-using-localizable-fields"></a>

Translated web content in localizable fields can be different structurally from the content displayed in the original language. Liferay does this so your changes don’t override content that has been already translated. To see how this works, follow these steps:

1. In Web Content, go to Structures and Click _Add_.
2. Create a structure using a repeatable fieldset. For example, add a Text and an Image field to your fieldset. Make them both required and click _Save_.
3. Add the fieldset to your new structure and make it repeatable.
4. In the upper left corner, click the flag icon and click _Manage Translations_.
5. Click _Add_ (![Add icon.](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png)) and add a language of your choice. Click _Done_.
6. Name your structure and click _Save_.
7. Now navigate to Web Content and click _New_. Select the structure you just created.
8. Enter a value for your text field and select an image.
9. In the upper left corner, click the flag icon and click the language previously chosen as your translation. Notice that the content in the fieldset has not changed. The same happens when you duplicate the fieldset using the default language; the secondary language’s content doesn’t change. Now change the translation back to the default language and name your web content. Click _Publish_.
10. Click _Actions_ (![Actions icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) next to your web content and click _Translate_.
11. On Translate From in the upper corner, select the language chosen in the structure creation. Click _Publish_.
12. Click your web content to enter edit mode.
13. In the upper left corner, click the flag icon. See that the secondary language has the Translated message next to it.

![See the Translated message next to the language.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/translating-pages-and-content/translating-web-content/images/03.png)

1. Using your default language, click _Duplicate_ (![Actions icon.](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) once more and enter a value for the text field and select an image.
2. In the upper left corner, click the flag icon and select the translated language. You can see that the content selected for the duplicated fieldset is not present when using this translation.

This behavior frees you to manage your translations independently from the original language.

### [Exporting and Importing Translations](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/translating-pages-and-content/translating-web-content#exporting-and-importing-translations) <a href="#exporting-and-importing-translations" id="exporting-and-importing-translations"></a>

NOTE

To translate content page experiences manually, users must have either View and Update permissions for content pages or Translation permissions for one or more languages. See [Managing Translation Permissions](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/translating-pages-and-content/managing-translation-permissions) for more information.

With Liferay, you can export web content for translation as `.xliff` or `.xlf` files. You can send these files to translators and then import the translations together as a `.zip` file or individually as `.xliff` or `.xlf` files.

IMPORTANT

Liferay supports both XLIFF 1.2 and 2.0 files. However, it may not support all features and capabilities those formats provide.

#### [Exporting Content for Translation](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/translating-pages-and-content/translating-web-content#exporting-content-for-translation) <a href="#exporting-content-for-translation" id="exporting-content-for-translation"></a>

1. Open the _Site Menu_ (![Site Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu.png)) and navigate to _Content & Data_ → _Web Content_.
2.  Click _Actions_ (![Actions icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) for the content you want to translate and select _Export for Translation_. This opens a modal window for configuring your export.

    ![Select Export for Translation.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/translating-pages-and-content/translating-web-content/images/04.png)

    In Liferay Portal 7.4 GA26+, you can select multiple articles and click the _Export for Translation_ button.

    ![Select multiple articles and click Export for Translation](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/translating-pages-and-content/translating-web-content/images/05.png)
3. Select an export format: _XLIFF 1.2_ or _XLIFF 2.0_.
4. Select the web content’s original language.
5.  Select the languages you want to translate to.

    ![Select the languages you want to translate the content into.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/translating-pages-and-content/translating-web-content/images/06.png)
6. Click _Export_.

Liferay generates a ZIP archive that contains an XLIFF file for each selected language. You can then use these files with compatible translation software.

#### [Importing Content Translations](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/translating-pages-and-content/translating-web-content#importing-content-translations) <a href="#importing-content-translations" id="importing-content-translations"></a>

1. Open the _Site Menu_ (![Site Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu.png)) and navigate to _Content & Data_ → _Web Content_.
2.  Click _Actions_ (![Actions icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) for the translated web content and select _Import Translation_.

    ![Select Import Translation.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/translating-pages-and-content/translating-web-content/images/07.png)

    In Liferay Portal 7.4 GA26+, you can import translations for multiple articles at once by clicking the _Actions_ button (![Actions Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) in the Application Bar and selecting _Import Translations_.

    ![Upload translations for multiple articles.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/translating-pages-and-content/translating-web-content/images/08.png)
3.  Select the `.xliff`, `.xlf`, and `.zip` files you want to import.

    ![Select the translation files you want to import.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/translating-pages-and-content/translating-web-content/images/09.png)

    IMPORTANT

    While Liferay supports importing XLIFF files created using the [Export for Translation](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/translating-pages-and-content/translating-web-content#exporting-content-for-translation) action, it cannot guarantee the successful import of files generated by other means.
4.  Click _Publish_.

    If successful, Liferay displays a success message with the imported files. The web content is updated with all changes included in the translation files. If workflow is enabled, this change must be approved as with other content updates.

    ![If successful, Liferay displays a success message with the imported files.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/translating-pages-and-content/translating-web-content/images/10.png)

    However, if errors occur during import, Liferay notifies you of the failing files and provides a downloadable CSV error report.

    ![If errors occur during import, Liferay notifies you of the failing files and provides a downloadable CSV error report.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/translating-pages-and-content/translating-web-content/images/11.png)

### [Translating Web Content Using Third Party Services](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/translating-pages-and-content/translating-web-content#translating-web-content-using-third-party-services) <a href="#translating-web-content-using-third-party-services" id="translating-web-content-using-third-party-services"></a>

If you’ve enabled Liferay’s integration with [Google Cloud Translation](https://cloud.google.com/translate/docs/setup), [Amazon Translate](https://docs.aws.amazon.com/translate/latest/dg/what-is.html), or [Microsoft Translator](https://docs.microsoft.com/en-us/azure/cognitive-services/translator/), you can use them to automatically translate web content. See [Using Third Parties for Translation](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/translating-pages-and-content/using-third-parties-for-translation) for more information.

\
