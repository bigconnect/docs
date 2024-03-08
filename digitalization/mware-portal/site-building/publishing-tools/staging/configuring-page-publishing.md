# Configuring Page Publishing

By default, publishing a child page to Live also publishes its parent pages. This prevents orphaned pages. Always publishing parent pages to Live, however, can affect performance and prematurely publish the changes to parent pages. If desired, you can configure this behavior to only publish your selected pages, instead of always including parent pages.

NOTE

This configuration only affects Staging. Manually exporting a Site to an LAR file always includes all parent pages.

Follow these steps to configure child/parent publishing for your system:

1. Open the _Global Menu_ (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)), click the _Control Panel_, and go to _System Settings_ → _Infrastructure_.
2. Click _Staging_ under Virtual Instance Scope in the left menu.
3.  Check or uncheck _Publish Parent Pages by Default_.

    If checked, parent pages are always published to Live.

    If unchecked, the parent page is only published if it doesn’t exist in Live yet. If the parent page was already published, only a reference element is added, which is used for the validation to make sure the parent is already in the Live site.
4. Click _Save_.

\
