# Translating Content Pages

MWARE Portal provides integrated tools for translating _Content Pages_, so you can create engaging, localized experiences for your global users. With these tools, you can manually translate Content Page experiences or import translations as XLIFF files.

IMPORTANT

When translating a Content Page, users can only translate a Page’s name and inline fragment text (e.g. HTML, Header, etc.). They cannot translate Widgets or mapped fragment fields.

If desired, you can enable a custom workflow to direct the review and publishing process for translations.

NOTE

The languages available for translation are determined by your instance’s localization settings. See Initial Instance Localization and Virtual Instance Localization for more information.

### Manually Translating Content Pages <a href="#manually-translating-content-pages" id="manually-translating-content-pages"></a>

NOTE

To manually translate Content Page experiences, users must have either _View_ and _Update_ permissions for Content Pages or Translation permissions for one or more languages. See Managing Translation Permissions for more information.

Follow these steps to manually translate Content Page experiences:

1. Open the _Site Menu_ (![Site Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu.png)), expand _Site Builder_, and go to _Pages_.
2.  Click the _Actions_ button (![Actions Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) for the Content Page you want to translate and select _Translate_.

    This redirects you to the translation interface, where you can view the content’s original text alongside your translation.

    The left column displays the language you’re translating from, and the right column provides editable fields you can use for your translation.

    ![Selecting Translate redirects you to the translation interface.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/translating-pages-and-content/translating-content-pages/images/01.png)
3.  Select the _experience_ and _language_ you want to translate.

    Users with Page Update permissions can translate the original text into any language.

    Users with translate permissions can only translate the origin text into languages for which they have permission.

    IMPORTANT

    Switching between Page experience discards all unsaved changes. To avoid losing work, save your changes before switching to a different experience.

    ![Select the experience and language you want to translate.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/translating-pages-and-content/translating-content-pages/images/02.png)
4. Enter your translation for available fields.
5.  Click _Publish_ or _Save as Draft_.

    Publishing your translations applies them to the Content Page and saves it as a draft.

    Also, if a workflow is enabled, publishing translations initiates the review process before they’re applied to the Page and saved as a draft.

    Alternatively, you can click _Save as Draft_ to apply your translations at a later time.

### Exporting and Importing Translations <a href="#exporting-and-importing-translations" id="exporting-and-importing-translations"></a>

NOTE

To export pages for translation or import XLIFF translations, you must have _View_ and _Update_ permissions for the selected page. See Managing Translation Permissions for more information.

You can export Content Page experiences for translation as `.xliff` or `.xlf` files. You can send these files to translators and then import the translations together as a `.zip` file or individually as `.xliff` or `.xlf` files.

IMPORTANT

The portal supports both XLIFF 1.2 and 2.0 files. However, it may not support all features and capabilities those formats provide.

#### Exporting Pages for Translation <a href="#exporting-pages-for-translation" id="exporting-pages-for-translation"></a>

Follow theses steps:

1. Open the _Site Menu_ (![Site Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu.png)), expand _Site Builder_, and go to _Pages_.
2.  Click the _Actions_ button (![Actions Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) for the Content Page you want to translate and select _Export for Translation_. This opens a modal window for configuring your export.

    ![Select Export for Translation.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/translating-pages-and-content/translating-content-pages/images/03.png)
3. Select an export format: _XLIFF 1.2_ or _XLIFF 2.0_.
4. Select the Content Page’s original language.
5. Select the languages you want to translate to.
6.  Select the experiences you want to translate.

    ![Configure the export.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/translating-pages-and-content/translating-content-pages/images/04.png)
7. Click _Export_.

The portal generates a ZIP archive that contains an XLIFF file for each selected experience. You can then use these files with compatible translation software.

#### Importing Page Translations <a href="#importing-page-translations" id="importing-page-translations"></a>

Follow theses steps:

1. Open the _Site Menu_ (![Site Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu.png)), expand _Site Builder_, and go to _Pages_.
2.  Click the _Actions_ button (![Actions Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) for the translated Content Page and select _Import Translation_.

    ![Select Import Translation.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/translating-pages-and-content/translating-content-pages/images/05.png)
3.  Select the `.xliff`, `.xlf`, and `.zip` files you want to import.

    ![Select the translation files you want to import.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/translating-pages-and-content/translating-content-pages/images/06.png)

    IMPORTANT

    While the portal supports importing XLIFF files created using the Export for Translation action, it cannot guarantee the successful import of files generated by other means.
4.  Click _Publish_.

    If import is successful, the portal displays a success message with the imported files. The translations are applied to the Content Page and the Page is saved as a draft. If workflow is enabled, the translations must be approved before they’re applied to the Page.

    ![If import is successful, the portal displays a success message with the imported files.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/translating-pages-and-content/translating-content-pages/images/07.png)

    However, if errors occur during import, the portal notifies you of the failing files and provides a downloadable CSV error report.

    ![If errors occur during import, the portal notifies you of the failing files and provides a downloadable CSV error report.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/translating-pages-and-content/translating-content-pages/images/08.png)

### Translating Content Pages Using Third Party Services <a href="#translating-content-pages-using-third-party-services" id="translating-content-pages-using-third-party-services"></a>

If you’ve enabled the portal’s integration with Google Cloud Translation, Amazon Translate, or Microsoft Translator, you can use them to automatically translate Web Content. See Using Third Parties for Translation for more information.

\
