# Auto Tagging

[Tagging assets](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/tags-and-categories/tagging-content-and-managing-tags) is a great way to organize content. Typically, the content creator manually applies tags during creation, but you can configure Liferay to tag content automatically. For example, you can scan an image on upload and apply tags that describe the image’s content.

NOTE

Currently, auto tagging only works for images, text-based documents, text-based web content, and blog entries. See [Auto Tagging Assets](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/tags-and-categories/auto-tagging/auto-tagging-assets) and [Auto Tagging Images](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/tags-and-categories/auto-tagging/auto-tagging-images) for more information.

Auto tagging is enabled by default. You can configure it at three levels:

**Global (System Settings)**: Enable or disable auto tagging for the Liferay server. For auto tagging to function on any level, it must be enabled globally. At the global level, you can also set a default configuration for all virtual instances.

**Virtual Instance (Instance Settings)**: Override auto tagging configurations set at the global level. This setting determines the default configuration for all instance sites.

**Site**: Override auto tagging configurations set at the global and instance levels.

By default, asset auto tagging is only triggered when content is created. Beginning with Liferay 7.4 U36/GA36, you can configure virtual instances to allow auto tag assets on updates as well. See [Virtual Instance Scope Configuration](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/tags-and-categories/auto-tagging/configuring-asset-auto-tagging#virtual-instance-scope-configuration) for more information.

### [Global Scope Configuration](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/tags-and-categories/auto-tagging/configuring-asset-auto-tagging#global-scope-configuration) <a href="#global-scope-configuration" id="global-scope-configuration"></a>

1. Open the _Global Menu_ (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)), click the _Control Panel_ tab, and go to _System Settings_.
2. In the Content and Data section, click _Assets_.
3.  Under System Scope, click _Asset Auto Tagging_.

    ![In Site Settings, click Assets and go to the Asset Auto Tagging tab under System Scope.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/tags-and-categories/auto-tagging/configuring-asset-auto-tagging/images/01.png)
4. Check the box to enable or disable Asset Auto Tagging globally.
5. Set the _Maximum Number of Tags_ that can be added automatically to an asset. Setting the value to `0` means there is no limit.
6. Click _Save_ when finished.

To set the default auto tagging configuration for all instances, select _Asset Auto Tagging_ under Virtual Instance Scope. The available settings are the same as those in the System Scope.

### [Virtual Instance Scope Configuration](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/tags-and-categories/auto-tagging/configuring-asset-auto-tagging#virtual-instance-scope-configuration) <a href="#virtual-instance-scope-configuration" id="virtual-instance-scope-configuration"></a>

When enabled globally, auto tagging is also enabled by default for each Virtual Instance. You can, however, disable or configure it for each instance.

Follow these steps to configure Auto Tagging at the Virtual Instance scope:

1. Open the _Global Menu_ (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)), click the _Control Panel_ tab, and go to _Instance Settings_.
2. In the Content and Data section, click _Assets_.
3.  Under Virtual Instance Scope, click _Asset Auto Tagging_.

    ![In Instance Settings, click Assets and go to the Asset Auto Tagging tab under Virtual Instance Scope.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/tags-and-categories/auto-tagging/configuring-asset-auto-tagging/images/02.png)
4. Check/Uncheck _Enable Asset Auto Tagging_.
5. Check/Uncheck _Update Auto Tags_. If unchecked, auto tagging is only triggered during asset creation and upload. If checked, auto tagging is triggered when assets are edited.
6. Set the _Maximum Number of Tags_ that can be added automatically to an asset. Setting the value to `0` means there is no limit.
7. Click _Save_ when finished.

Once saved, this determines the default configuration for the current virtual instance. If you’ve enabled the _Update Auto Tags_ feature, editing supported assets triggers auto tagging. If desired, you can disable this behavior for existing content items.

![You can disable update auto tagging for individual content items if desired.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/tags-and-categories/auto-tagging/configuring-asset-auto-tagging/images/03.png)

### [Site Scope Configuration](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/tags-and-categories/auto-tagging/configuring-asset-auto-tagging#site-scope-configuration) <a href="#site-scope-configuration" id="site-scope-configuration"></a>

1. Open the _Site Menu_ (![Site Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu.png)), expand _Configuration_, and go to _Site Settings_.
2.  For 7.4+, go to _Assets_ under Content and Data and click the _Asset Auto Tagging_ tab.

    ![Go to Assets under Content and Data and click the Asset Auto Tagging tab.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/tags-and-categories/auto-tagging/configuring-asset-auto-tagging/images/04.png)

    For earlier versions, go to the _General_ tab and expand the _Asset Auto Tagging_ section.
3. Use the toggle to enable or disable auto tagging for the site.
4. Click _Save_.

\
