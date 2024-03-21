# Auto Tagging Assets

You can manually tag most assets, such as Message Boards, Blogs, Documents, Images, and Web Content Articles. You can tag certain assets, however, automatically when they are created. Tags applied automatically are in only English.

Auto tagging is only available for text-based documents, text-based web content, and blog entries, as well as images.

Text Auto Tagging is disabled by default. You must enable Auto Tagging in the portal and then enable a text auto tagging provider. Finally, you must choose the asset to auto-tag. There are two text auto-tagging providers available:

* **Google Cloud Natural Language Text Auto Tagging**: Uses the Google Cloud Natural Language API to analyze and automatically tag content.
* **OpenNLP Text Auto Tagging**: Uses the open source Apache OpenNLP library to analyze and automatically tag portal content. Three models are used: location name finder, organization finder, and person name finder.

TIP

Use this provider with caution because its accuracy may be limited.

### Configuring Google Cloud Natural Language Text Auto Tagging <a href="#configuring-google-cloud-natural-language-text-auto-tagging" id="configuring-google-cloud-natural-language-text-auto-tagging"></a>

1. Click the _Global Menu_ → _Control Panel_
2.  Click _System Settings_.

    ![Navigate to the System Settings.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/tags-and-categories/auto-tagging/auto-tagging-assets/images/01.png)
3.  Click _Assets_ under _Content and Data_.

    ![Click on Assets.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/tags-and-categories/auto-tagging/auto-tagging-assets/images/02.png)
4. Under _VIRTUAL INSTANCE SCOPE_, click _Google Cloud Natural Language Text Auto Tagging_.
5. Fill out the form.
6. Click _Save_ when finished.

The form asks for this information:

**API Key:** The API key to use for the Google Cloud Natural Language API. For more information, see Google’s documentation on API keys.

**Classification Endpoint Enabled:** Whether to enable auto tagging of text using the Google Cloud Natural Language API Classification endpoint.

**Confidence:** Set the classifier’s confidence of the category. This number represents how certain the classifier is that this category represents the given text.

**Entity Endpoint Enabled:** Whether to enable auto tagging of text using the Google Cloud Natural Language API Entity endpoint.

**Salience:** The salience score for an entity provides information about the importance or centrality of that entity to the entire text.

**Enable Google Cloud Natural Language Text Auto Tagging For:** Choose the asset types to be auto-tagged. Select _Document_, _Blogs Entry_, or _Web Content Article_. To add multiple asset types, click the _Add_ icon (![Add](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png)) and select the asset type from the menu. You can delete any additional asset types by clicking the Trash icon (![Trash](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-trash.png)).

![Enter your settings.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/tags-and-categories/auto-tagging/auto-tagging-assets/images/03.png)

The Google Cloud Natural Language provider has been configured.

If there are multiple virtual instances on the server, you can override these settings for each instance from _Control Panel_ → _Instance Settings_ → _Assets_.

### Configuring OpenNLP Text Auto Tagging <a href="#configuring-opennlp-text-auto-tagging" id="configuring-opennlp-text-auto-tagging"></a>

1. Click the _Global Menu_ → _Control Panel_
2. Click _System Settings_.
3. Click _Assets_ under _Content and Data_.
4. Under _VIRTUAL INSTANCE SCOPE_, click _OpenNLP Text Auto Tagging_.
5. Fill out the form.
6. Click _Save_ when finished.

The form asks for this information:

**Confidence Threshold:** Set the minimum confidence threshold (from 0 to 1, where 1 is the highest confidence). Higher values yield fewer tags because the provider needs more confidence before it applies a tag. Lower values yield more tags.

**Enable OpenNLP Text Auto Tagging For:** Choose the asset types to be auto-tagged. Use the menu to select _Document_, _Blogs Entry_, or _Web Content Article_. To add multiple asset types, click the _Add_ icon (![Add](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png)) and select the asset type from the menu. You can delete any additional asset types by clicking the Trash icon (![Trash](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-trash.png)).

![Enter your settings.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/tags-and-categories/auto-tagging/auto-tagging-assets/images/04.png)

The OpenNLP provider has been configured.

If there are multiple virtual instances on the server, you can override these settings for each instance from _Control Panel_ → _Instance Settings_ → _Assets_.

\
