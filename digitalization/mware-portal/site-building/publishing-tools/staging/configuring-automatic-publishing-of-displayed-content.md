# Configuring Automatic Publishing of Displayed Content

By default, only content explicitly included in a publishing process is published to Live. If desired, you can configure your instance to always publish content displayed using the following widgets: Dynamic Data Lists Display, Form, Web Content Display, and Wiki Display. This can help ensure displayed content is always up to date in Live.

Follow these steps to configure this Staging behavior:

1. Open the _Global Menu_ (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)), click the _Control Panel_, and go to _System Settings_ → _Infrastructure_.
2. Click _Staging_ under Virtual Instance Scope in the left menu.
3.  Check or uncheck _Publish Displayed Content by Default_.

    When enabled, displayed content is republished for the following widgets, regardless of the publishing process’s configuration: Dynamic Data Lists Display, Form, Web Content Display, and Wiki Display.

    When disabled, only content included in the publishing process’s configuration is published.

    TIP

    Keep in mind that always republishing displayed content to Live can be unnecessary and slow down publishing.
4. Click _Save_.

![Check or uncheck Publish Displayed Content by Default.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/publishing-tools/staging/configuring-automatic-publishing-of-displayed-content/images/01.png)

### Configuring Publishing for Asset Publisher <a href="#configuring-publishing-for-asset-publisher" id="configuring-publishing-for-asset-publisher"></a>

By default, Asset Publisher does not export displayed content to Live automatically. This is because the widget’s content is usually available in Live. The portal provides additional configurations for content items displayed using the Asset Publisher widget.

Follow these steps to enable dynamic or manual export for the Asset Publisher widget:

1. Open the _Global Menu_ (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)), click the _Control Panel_, and go to _System Settings_ → _Assets_.
2. Click _Asset Publisher_ under System Scope in the left menu.
3.  Check/Uncheck _Enable Dynamic Export_.

    When enabled, Asset Publisher exports include all related assets within the _Dynamic Export Limit_.
4.  Check _Enable Manual Export_.

    When enabled, users can select which related assets to include in the export manually.
5. Click _Save_.

\
