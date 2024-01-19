# Configuring Asset Auto Tagging

Tagging assets is a great way to organize content. Typically, the content creator applies tags while creating the content. It’s also possible, however, to tag content automatically. For example, the portal can scan an image on upload and apply tags that describe the image’s content. This lets you leverage tags without requiring content creators to apply them manually.

**Note:** Auto-tagging currently works only for images, text-based documents, text-based web content, and blog entries.

Here, you’ll learn how to configure asset auto-tagging in general. This is required prior to configuring auto-tagging for specific asset types, which is documented separately:

* Auto Tagging Images
* Auto Tagging Text

### Configuration Levels <a href="#configuration-levels" id="configuration-levels"></a>

Auto-tagging is enabled by default. You can configure it at three levels:

**Global (System):** For auto-tagging to function on any level, it must be enabled globally. You can also set the default auto-tagging configuration for every portal instance.

**Instance:** When enabled globally, auto-tagging is also enabled by default for each portal instance. However, you can override the global auto-tagging configuration on a per-instance basis.

**Site:** When enabled for an instance, auto-tagging is also enabled by default for all that instance’s sites. You can disable it for specific sites.

#### GLOBAL CONFIGURATION <a href="#global-configuration" id="global-configuration"></a>

Follow these steps to configure auto tagging globally:

1. Go to _Control Panel_ → _Configuration_ → _System Settings_ → _Assets_.
2. Under _SYSTEM SCOPE_, select _Asset Auto Tagging_.
3.  The following settings are available:

    **Enable Auto Tagging of Assets:** Whether asset auto tagging is enabled.

    **Maximum Number of Tags:** The maximum number of tags that can be automatically applied to each asset. The default value of `0` means that there is no limit.
4. Click _Save_ to save your changes.

To set the default auto-tagging configuration for all instances, select _Asset Auto Tagging_ under _VIRTUAL INSTANCE SCOPE_. The available settings are exactly the same as those in the SYSTEM SCOPE.

![Figure 1: You can configure auto tagging globally in the Assets section of System Settings.](https://liferay-support.zendesk.com/hc/article\_attachments/360029927212/auto-tagging-global.png)

#### INSTANCE-LEVEL CONFIGURATION <a href="#instance-level-configuration" id="instance-level-configuration"></a>

When enabled globally, auto-tagging is also enabled by default for each instance. You can, however, disable or configure it for each instance.

Follow these steps to configure auto tagging on the instance level:

1. Go to _Control Panel_ → _Configuration_ → _Instance Settings_ → _Assets_.
2. Under _VIRTUAL INSTANCE SCOPE_, select _Asset Auto Tagging_.
3. The settings here are identical to those in the global configuration, but apply only to the current instance.
4. Click _Save_ to save your changes.

![Figure 2: You can also configure auto tagging for each instance.](https://liferay-support.zendesk.com/hc/article\_attachments/360029927232/auto-tagging-instance.png)

#### SITE-LEVEL CONFIGURATION <a href="#site-level-configuration" id="site-level-configuration"></a>

When enabled for an instance, auto-tagging is also enabled by default for all that instance’s sites. You can, however, enable or disable it for each site.

Follow these steps to configure auto tagging for a site:

1. Open the _Menu_ (![Product Menu](https://liferay-support.zendesk.com/hc/article\_attachments/360029927252/icon-menu.png)), click your site’s name, and navigate to _Configuration_ → _Settings_.
2. In the _General_ tab, expand the _Asset Auto Tagging_ section. Use the toggle to enable or disable auto tagging for the site.
3. Click _Save_ to save your changes.

![Figure 3: You can enable or disable auto-tagging for a site.](https://liferay-support.zendesk.com/hc/article\_attachments/360029911411/auto-tagging-site.png)
