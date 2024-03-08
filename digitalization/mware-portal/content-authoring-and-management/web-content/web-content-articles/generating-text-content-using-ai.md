# Generating Text Content Using AI

Web content provides integration with OpenAI’s ChatGPT engine using the AI Creator tool. With an OpenAI API key configured for your instance or site, you can use the AI Creator feature in the Web Content editor. Once configured, you can begin generating content to work with, in a few clicks.

### [Enabling AI Creator](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/web-content/web-content-articles/generating-text-content-using-ai#enabling-ai-creator) <a href="#enabling-ai-creator" id="enabling-ai-creator"></a>

IMPORTANT

ChatGPT Integration for Web Content is currently behind a release feature flag. Read [Feature Flags](https://learn.liferay.com/web/guest/w/dxp/system-administration/configuring-liferay/feature-flags) for more information.

To begin, you must add an API key to authenticate your access to ChatGPT. You can [add it to a Liferay instance](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/web-content/web-content-articles/generating-text-content-using-ai#adding-an-api-key-to-a-liferay-instance) through _Instance Settings_ or add it [to a specific site](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/web-content/web-content-articles/generating-text-content-using-ai#adding-an-api-key-to-a-specific-site).

#### [Adding an API Key to a Liferay Instance](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/web-content/web-content-articles/generating-text-content-using-ai#adding-an-api-key-to-a-liferay-instance) <a href="#adding-an-api-key-to-a-liferay-instance" id="adding-an-api-key-to-a-liferay-instance"></a>

1. Open the _Global Menu_( ![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png) ) and navigate to the _Control Panel_ tab → _Instance Settings_ (under Configuration).
2. Click _AI Creator_ under _Content and Data_.
3. Paste your OpenAI API key into the _API Key_ field.
4. Click _Save_.

#### [Adding an API Key to a Specific Site](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/web-content/web-content-articles/generating-text-content-using-ai#adding-an-api-key-to-a-specific-site) <a href="#adding-an-api-key-to-a-specific-site" id="adding-an-api-key-to-a-specific-site"></a>

By adding an API key in the scope of a specific site, you can use different keys for each site or disable a key in a specific site.

1. Open the _Product Menu_ ( ![Product Menu icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu.png) ) and select _Configuration_ → _Site Settings_.
2. Click _AI Creator_ under _Content and Data_.
3. Paste your OpenAI API key into the _API Key_ field.
4. Click _Save_.

NOTE

You can enable or disable the AI Creator button by toggling the Enable OpenAI to Create Content checkbox. If you choose to disable the feature, the button does not appear in the Web Content editor.

### [Using AI Creator](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/web-content/web-content-articles/generating-text-content-using-ai#using-ai-creator) <a href="#using-ai-creator" id="using-ai-creator"></a>

After [enabling the feature flag and configuring the API Key](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/web-content/web-content-articles/generating-text-content-using-ai#enabling-ai-creator), AI Creator becomes available in the web content editor. To begin generating content, follow these steps:

1. Open the _Global Menu_( ![Global Menu icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png) ) and select your site on the right.
2. Open the _Product Menu_ ( ![Product Menu Icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu.png) ) and select the _Web Content_ app under Content & Data.
3. Click _Add_ ( ![Add icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png) ) to create a Basic Web Content article.
4.  Click _AI Creator_.

    ![After enabling the feature flag, you can see the AI Creator button in the Web Content editor.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/web-content/web-content-articles/generating-text-content-using-ai/images/01.png)
5. In the AI Creator modal window, fill in these three fields:
   * **Description**: The desired content (e.g. “A blog post about the sleeping habits of cats”).
   * **Tone**: The tone for your text. There are five options available: Neutral, Casual, Friendly, Formal, and Academic.
   * **Word Count**: An approximate word count for your text.
6. Click _Create_.
7.  (Optional) If you don’t like the AI-generated content, you can click _Try Again_ to regenerate the content.

    ![You can choose the content, the tone, and the approximate number of words in your content. You can also ask the AI Creator to regenerate the content if you don't like it.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/web-content/web-content-articles/generating-text-content-using-ai/images/02.png)

    IMPORTANT

    Each request to generate content consumes [OpenAI API tokens](https://help.openai.com/en/articles/4936856-what-are-tokens-and-how-to-count-them). When your limit is reached, a generic error message will appear.
8. Click _Add_ to insert the content into your web content article.

You can now begin working with your generated content.

\
