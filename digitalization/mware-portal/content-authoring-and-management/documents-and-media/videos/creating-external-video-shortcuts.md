# Creating External Video Shortcuts

With Liferay’s Documents and Media application, you can save references to videos hosted on external platforms. These references can then be used to [embed videos](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/documents-and-media/videos/embedding-videos-into-liferay-assets-and-pages) into Liferay assets or Site Pages. YouTube, Facebook, Vimeo, and Twitch are supported out of the box, but you can extend this feature to [support custom video sources](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/documents-and-media/developer-guide/creating-video-shortcut-providers).

Follow these steps to create an external video shortcut:

1.  Navigate to the Documents and Media application in a Site or Asset Library.

    NOTE

    Shortcuts created in a Site are scoped to that Site, while shortcuts created in an Asset Library can be shared across multiple Sites. See [Asset Libraries Overview](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/asset-libraries) for more information.
2.  Click the _Add_ button (![Add Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png)), and select _External Video Shortcut_.

    ![Add a new External Video Shortcut.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/videos/creating-external-video-shortcuts/images/01.png)
3.  Enter a video URL from a supported platform: YouTube, Vimeo, Facebook, or Twitch.

    Videos from YouTube and Vimeo display a URL thumbnail. Facebook and Twitch videos do not.

    ![Enter a Video URL.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/videos/creating-external-video-shortcuts/images/02.png)
4.  Enter a title for the shortcut.

    This field is auto-populated for YouTube and Vimeo. Facebook and Twitch require manual input.
5.  (Optional) Select a Display Page Template for the video.

    The selected template displays the video at a unique friendly URL and defines the display page’s layout. See [Creating and Managing Display Page Templates](https://learn.liferay.com/web/guest/w/dxp/site-building/displaying-content/using-display-page-templates/creating-and-managing-display-page-templates) for more information.

    ![Select a Display Page Template for the video.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/videos/creating-external-video-shortcuts/images/03.png)
6. (Optional) Categorize the video shortcut using [tags and categories](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/tags-and-categories/organizing-content-with-categories-and-tags).
7. (Optional) Select related assets for the video shortcut.
8. (Optional) Configure permissions for the video shortcut.
9.  Click _Publish_ when finished.

    You can view and manage external video shortcuts like any other Documents and Media asset:

    ![View and manage external video shortcuts in Documents and Media.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/videos/creating-external-video-shortcuts/images/04.png)

Once created, External Video Shortcuts can be embedded inside most Liferay assets. See [Embedding Videos into Liferay Assets and Pages](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/documents-and-media/videos/embedding-videos-into-liferay-assets-and-pages) for more information.

\
