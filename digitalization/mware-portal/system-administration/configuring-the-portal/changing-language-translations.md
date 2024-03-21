# Changing Language Translations

Use the Language Override tool to override existing translations or add new language keys and translations. Note that these overrides take place at the Instance level of Configuration Scope. For previous portal versions, see Overriding Global Language Translations.

### Overriding a Translation <a href="#overriding-a-translation" id="overriding-a-translation"></a>

1. Open the Global Menu (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)). Navigate to _Control Panel_ → _Configuration_ → _Language Override_.
2.  Use the search bar to find the translation you wish to override. For example, if you want to override `home` found at the top of the Product Menu, search for the term `home`.

    ![Search for the term home in the Language Override UI.](https://resources.learn.liferay.com/images/dxp/latest/en/system-administration/configuring-liferay/changing-language-translations/images/01.png)

    Note, the search bar searches language keys for the selected locale’s translations. Use the locale selector to switch between languages. For example, select `en-US` to search within US English translations.
3. Locate and click on the translation. A new window opens. You can also click the _Actions_ (![Actions icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) → _Edit_ to the right of the translation.
4.  Enter a new translation for each language you want to override. Click _Save_ at the bottom of the page.

    ![Enter a new translation for each language you want to override.](https://resources.learn.liferay.com/images/dxp/latest/en/system-administration/configuring-liferay/changing-language-translations/images/02.png)
5.  The translation for `home` is now overridden.

    ![The translation for home is now overridden.](https://resources.learn.liferay.com/images/dxp/latest/en/system-administration/configuring-liferay/changing-language-translations/images/03.png)

### Modifying a Translation Override <a href="#modifying-a-translation-override" id="modifying-a-translation-override"></a>

1. Open the Global Menu (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)). Navigate to _Control Panel_ → _Configuration_ → _Language Override_.
2.  Use the search bar to find the override you wish to modify. To filter the results for only the translations that have overrides, click _Filter and Order_ and select _Any Language_. Or click _Selected Language_ to only show overrides within the locale selected.

    Note existing overrides are indicated under each language key. For example, “Languages With Override: en\_US, ja\_JP”.
3.  Select the translation you wish to modify. A new window opens. You can also click the _Actions_ (![Actions icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) → _Edit_ to the right of the translation.

    If overrides exist, the _Actions_ icon (![Actions icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) also has an option to remove translation overrides. Click _Remove all translation overrides_ and click _OK_ in the pop-up window to remove all overrides. If an override exists in the locale you have selected, there’s an additional option to remove just that locale’s translation (e.g. _Remove translation override for en\_US_ if US English is selected).

    ![The Actions icon has an option to remove translation overrides.](https://resources.learn.liferay.com/images/dxp/latest/en/system-administration/configuring-liferay/changing-language-translations/images/04.png)
4. Enter your changes and click _Save_ at the bottom of the page. If you wish to remove all overrides and revert back to the original translations, click _Clear All Overrides_.

### Adding a Language Key and Translation <a href="#adding-a-language-key-and-translation" id="adding-a-language-key-and-translation"></a>

1. Open the Global Menu (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)). Navigate to _Control Panel_ → _Configuration_ → _Language Override_.
2. Click _Add_ (![Add button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png)) and a new window opens.
3. Enter a key and at least one language translation. Note, the maximum length is 1,000 characters for the key. Click _Save_ when finished.

### Importing and Exporting Language Translations <a href="#importing-and-exporting-language-translations" id="importing-and-exporting-language-translations"></a>

Language overrides can be easily imported or exported with language properties files.

#### Importing Language Translations <a href="#importing-language-translations" id="importing-language-translations"></a>

1. To import, click the _Actions_ (![Actions icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) to the left of the Global Menu icon.
2.  Select _Import Translations_ and a new window will open.

    ![Select import translations and a new window will open.](https://resources.learn.liferay.com/images/dxp/latest/en/system-administration/configuring-liferay/changing-language-translations/images/05.png)
3. Use the drop-down menu to select the language.
4. Click _Choose File_ and select the `Language.properties` file that contains your overrides. Note, that you must supply a separate file for each language you wish to override. Each language has its own language properties file containing keys for its language.
5. Click _Save_ and the overrides are now saved.

#### Exporting Language Translations <a href="#exporting-language-translations" id="exporting-language-translations"></a>

1. To export, click the _Actions_ (![Actions icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) to the left of the Global Menu icon.
2. Select _Export_ and a ZIP file containing `Language.properties` files for each language with overrides will begin downloading.
