# Recycle Bin

An integrated Recycle Bin can temporarily store content and data you want to remove from your instance before making that removal permanent. By default, the Recycle Bin is enabled instance-wide, but you can disable it at any time for individual Sites.

While enabled, the Delete action moves assets to the recycle bin for recyclable assets (i.e., folders, web content, blogs, bookmarks, documents, media, message boards, and wikis).

![The folder is moved to Recycle Bin and not deleted immediately.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/recycle-bin/recycle-bin-overview/images/01.png)

Once recycled, assets are retained for a specified period of time, during which Users can restore or permanently delete them. When an asset has remained in the Recycle Bin for longer than the specified retention period, it is automatically deleted from your instance. The default retention period is 43,200 minutes (i.e., 30 days).

![There is an option to delete or restore a file.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/recycle-bin/recycle-bin-overview/images/02.png)

If the Recycle Bin is disabled on a Site, deleted assets are permanently deleted and cannot be restored.

![Delete an asset.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/recycle-bin/recycle-bin-overview/images/03.png)

### Recycle Bin and Staging <a href="#recycle-bin-and-staging" id="recycle-bin-and-staging"></a>

If you have a Staging environment enabled for your Site, you have a separate Recycle Bin for both the Staging environment and the Live environment. This prevents staged assets and live assets from mixing.

To move an asset to the Recycle Bin, first switch to the environment containing the asset. Click on Staging or Live.

![Check which environment you are working in. Click on Staging or Live to switch.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/recycle-bin/recycle-bin-overview/images/04.png)

Using the Recycle Bin has the same functionality whether you are working in a Staging environment or a Live environment.

Note that the Recycle Bin is enabled for different asset types when the Staging environment is initially configured. See Managing Data and Content Types in Staging to make any changes.

WARNING

Check your Recycle Bin before enabling or disabling a Staging environment. You may end up losing assets that are already in the Recycle Bin.

\
