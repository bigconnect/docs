# Auto Tagging Images

Once asset auto tagging has been enabled, you can also enable image auto tagging. Image auto tagging tags images uploaded to the Documents and Media Library.

TIP

Currently, tags applied automatically are English only.

By default, Image Auto Tagging is disabled by default. In addition to enabling Auto Tagging, you must also configure an image tagging provider. Three providers are available:

* **TensorFlow:** An open-source library that provides machine learning capabilities. TensorFlow image auto-tagging is based on TensorFlow’s `LabelImage` sample for Java, and uses the Inception5h model. Use this with caution, since its accuracy is limited.
* **Google Cloud Vision:** Uses the Google Cloud Vision API to automatically tag images.
* **Microsoft Cognitive Services:** Uses Microsoft Cognitive Services to automatically tag images.

![The tags freight car and electric locomotive were automatically applied to this image.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/tags-and-categories/auto-tagging/auto-tagging-images/images/01.png)

### Configuring TensorFlow Image Auto Tagging <a href="#configuring-tensorflow-image-auto-tagging" id="configuring-tensorflow-image-auto-tagging"></a>

1. Click the _Global Menu_ → _Control Panel_
2.  Click _System Settings_.

    ![Navigate to the System Settings.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/tags-and-categories/auto-tagging/auto-tagging-images/images/02.png)
3.  Click _Assets_ under _Content and Data_.

    ![Click on Assets.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/tags-and-categories/auto-tagging/auto-tagging-images/images/03.png)
4. Under _VIRTUAL INSTANCE SCOPE_, click _TensorFlow Image Auto Tagging_.
5. Fill out the form.
6. Click _Save_ when finished.

The form asks for this information:

**Enable TensorFlow Image Auto Tagging:** Check this box to select whether image auto-tagging is enabled by default in any instance that has asset auto tagging enabled.

**Confidence Threshold:** TensorFlow assigns a confidence level between 0 and 1 for each tag, where 1 is the highest confidence and 0 is the lowest. This field sets the minimum confidence level that TensorFlow needs to apply a tag. Higher values yield fewer tags because TensorFlow needs more confidence before it applies a tag. Lower values yield more tags.

![Configure TensorFlow image auto-tagging for your portal instances.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/tags-and-categories/auto-tagging/auto-tagging-images/images/04.png)

#### Optimizing TensorFlow Auto Tagging Performance <a href="#optimizing-tensorflow-auto-tagging-performance" id="optimizing-tensorflow-auto-tagging-performance"></a>

1. Click the _Global Menu_ → _Control Panel_
2. Click _System Settings_.
3. Click _Assets_.
4. Under _SYSTEM SCOPE_, click _TensorFlow Image Auto Tagging Process_.
5. Fill out the form.
6. Click _Save_ to apply the settings.

The form asks for this information:

**Maximum Number of Relaunches:** The maximum number of times the process is allowed to crash before it is permanently disabled.

**Maximum Number of Relaunches Time Interval:** The time in seconds after which the counter is reset.

![You can fine tune the process that runs the TensorFlow image auto tagging in the portal.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/tags-and-categories/auto-tagging/auto-tagging-images/images/05.png)

The TensorFlow provider has been configured.

### Configuring Google Cloud Vision <a href="#configuring-google-cloud-vision" id="configuring-google-cloud-vision"></a>

1. Click the _Global Menu_ → _Control Panel_
2. Click _System Settings_.
3. Click _Assets_.
4. Under _VIRTUAL INSTANCE SCOPE_, click _Google Cloud Vision Image Auto Tagging_.
5. Enter your Google Cloud Vision API Key. For more information, see Google’s documentation on API keys.
6.  Check the _Enabled_ box to enable Google Cloud Vision.

    ![The Google Cloud Vision provider requires an API key.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/tags-and-categories/auto-tagging/auto-tagging-images/images/06.png)
7. Click _Save_ when finished.

### Configuring Microsoft Cognitive Services <a href="#configuring-microsoft-cognitive-services" id="configuring-microsoft-cognitive-services"></a>

1. Click the _Global Menu_ → _Control Panel_
2. Click _System Settings_.
3. Click _Assets_
4. Under _VIRTUAL INSTANCE SCOPE_, click _Microsoft Cognitive Services Image Auto Tagging_.
5. Fill out the form.
6.  Check the _Enabled_ box.

    ![The Microsoft Cognitive Services provider requires an API key and an endpoint.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/tags-and-categories/auto-tagging/auto-tagging-images/images/07.png)
7. Click _Save_ when finished.

The form asks for this information:

**API Key:** Your API key for the Computer Vision API V2.

**API Endpoint:** The endpoint for the Computer Vision API V2.

The Microsoft Cognitive Services provider has been configured. For more information, see the Microsoft Cognitive Services documentation.

If you have more than one virtual instance, you can configure each instance individually. Navigate to the _Control Panel_ → _Instance Settings_. Click _Assets_ then configure the provider there.
