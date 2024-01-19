# Other Content Options

Here are some options and tools that you can use to enhance your content and user experience.

### Localizing Content <a href="#localizing-content" id="localizing-content"></a>

When you create a new web content article, you can choose a default language. First, you must change the system configuration to enable the option to change the default language.

1. Go to the _Control Panel_ → _Configuration_ → _System Settings_.
2. Locate _Web Content_ → _Administration_ by scrolling or using the search bar.
3. Check the box labeled _Changeable Default Language_.
4. Click _Save_.

You must now add translations for any languages you need. Adding translations works like this:

1. Open a web content article.
2. Click the flag icon with a country code on it next to any localizable web content field.
3. Select a language from the list.

When you select a language, all fields in the article switch to the new language. To create the new translation, fill in the fields in the selected language and publish the article.

![Figure 1: Adding a translation to an article works like adding the default translation.](https://liferay-support.zendesk.com/hc/article\_attachments/360030159591/web-content-translation.png)

**Note:** To view localizable fields in a given language, you must have your Portal set to that language. This includes friendly URLs for the web content as well. When you navigate to the localized friendly URL (e.g. `http://localhost:8080/web/guest/-/espanol`), the web content is always displayed in the current language. You can change the language with the Language Selector app.

You can modify the language translation list by inserting `locales.enabled=` followed by your preferred languages in your `portal-ext.properties` file. For example, `locales.enabled=ar_SA,nl_NL,hi_IN` offers _Arabic (Saudi Arabia)_, _Dutch (Netherlands)_, and _Hindi (India)_.

**Warning:** If you switch your Site’s default language (e.g., via friendly URL), but do not have the necessary translations for localizable fields, your Site’s language values are used from the old default language. Therefore, you should change the default language of your Site _only_ when you have translated values for all localizable entities.

When you create a new web content structure, each field you create has a _Localizable_ checkbox displayed next to it. This enables you to control what can and can’t be changed in the translation process. For example, if you don’t want images or content titles to be changed when the content is translated, you can make sure those fields aren’t localizable. When you follow the steps above to localize content, only fields within the structure that had the _Localizable_ box checked appear within the translation window.

### Xuggler for Embedding Video <a href="#xuggler-for-embedding-video" id="xuggler-for-embedding-video"></a>

Xuggler is a tool which generates video previews and makes it possible to embed videos from your Documents and Media library in web content and elsewhere on the site. To enable Xuggler,

1. Navigate to the _Control Panel_.
2. Click on _Configuration_ → _Server Administration_ → _External Services_.
3.  Scroll to the bottom and click _Install_ in the _Xuggler_ section.

    This downloads the necessary libraries and prompts you to restart the server to enable Xuggler.
4. After the server restarts, you can enable Xuggler from the same page.

Once Xuggler has been installed and enabled, you can embed a video or audio file in a web content article the same way you added images previously.

![Figure 2: If youve installed and enabled Xuggler from the Server Administration → External Tools section of the Control Panel, you can add audio and video to your web content!](https://liferay-support.zendesk.com/hc/article\_attachments/360030190472/web-content-audio-video.png)

### XML Format Downloads <a href="#xml-format-downloads" id="xml-format-downloads"></a>

Tools like the Resource Importer and Site Initiators can be deployed to build a site almost instantly. Before you can use them to import Web Content, however, you first need to have the content exported individually in XML format. To export the content,

1. Go to _Site Administration_ → _Content & Data_ → _Web Content_.
2. Start editing the article you want to download.
3. Click the _Options_ icon (![Options](https://liferay-support.zendesk.com/hc/article\_attachments/360030159571/icon-options.png)) in the top right of the page and select _View Source_.

This displays the raw XML source of the article. You can copy this content to save into an XML file locally.

![Figure 3: The View Source button is available from the Options button.](https://liferay-support.zendesk.com/hc/article\_attachments/360030159651/web-content-download.png)

### Subscribing to Content <a href="#subscribing-to-content" id="subscribing-to-content"></a>

An administrator or web content writer can subscribe to an article or folder to follow changes being made to it.

1. Go to _Content & Data_ → _Web Content_ for your Site.
2. Click _Actions_ (![Actions](https://liferay-support.zendesk.com/hc/article\_attachments/360030159611/icon-app-options.png)) → _Subscribe_ next to the article or folder you want to follow.

Anytime an asset that you follow is modified, you receive an email notifying you of the change.

![Figure 4: Click the Subscribe icon in the web content entitys Options menu to begin receiving web content notifications.](https://liferay-support.zendesk.com/hc/article\_attachments/360030159631/web-content-subscribe.png)

That’s pretty much all there is to basic content creation. Whole sites have been created this way. But if you want to take advantage of the full power of the portal’s WCM, you’ll want to use structures and templates or Fragments. You’ll cover these topics next.

### Organizing Structure Names <a href="#organizing-structure-names" id="organizing-structure-names"></a>

By default, when you select a structure to add a new Web Content article, the structures are ordered by their IDs, not their names. This can be confusing, but—never fear—there’s a configuration property to sort them alphabetically.

![Figure 5: The default ordering for Web Content Structures can yield confusing results.](https://liferay-support.zendesk.com/hc/article\_attachments/360030190492/web-content-default-order.png)

To enable this property for Site Administration,

1. Go to _Configuration_ → _System Settings_ → _Web Content_ → _Web Content Administration_.
2. Check the box labeled _Journal Browse by Structures Sorted by Name_.

![Figure 6: Web Content Administration will now display structures in alphabetical order.](https://liferay-support.zendesk.com/hc/article\_attachments/360030159551/web-content-admin-alphabetical.png)

You can also set this property for the Web Content Display widget. To enable this property for the Web Content Display,

1. Go to _Configuration_ → _System Settings_ → _Web Content_ → _Web Content Display_.
2. Check the box labeled _Sort Structures by Name_.

After this option is checked, the structures are sorted alphabetically. Note that enabling this property can degrade performance with large structure libraries.
