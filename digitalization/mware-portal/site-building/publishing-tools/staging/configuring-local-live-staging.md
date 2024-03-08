# Configuring Local Live Staging

With Local Live staging, both your staging and live environments are hosted on the same Liferay server. When enabled, Liferay DXP creates a local clone of your site or asset library that serves as the staging environment, while the original becomes your live environment. Both environments share the same JVM, database, selected application data, and configurations (e.g., portal properties).

![Local staging hosts both environments on the same server.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/publishing-tools/staging/configuring-local-live-staging/images/01.png)

### [Setting Up Local Live Staging for Sites](https://learn.liferay.com/web/guest/w/dxp/site-building/publishing-tools/staging/configuring-local-live-staging#setting-up-local-live-staging-for-sites) <a href="#setting-up-local-live-staging-for-sites" id="setting-up-local-live-staging-for-sites"></a>

1.  Go to _Site Menu_ → _Publishing_ → _Staging_.

    ![Go to Staging in the Site Menu.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/publishing-tools/staging/configuring-local-live-staging/images/02.png)
2.  Select _Local Live_, which reveals additional fields for _Page Versioning_ and _Staged Content_.

    ![Select Local Live.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/publishing-tools/staging/configuring-local-live-staging/images/03.png)
3.  (For Sites Only) Select whether you want _Page Versioning_ enabled.

    ![Enable Page Versioning.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/publishing-tools/staging/configuring-local-live-staging/images/04.png)
4.  Select the data and content types you want to stage.

    ![Select the data and content types you want to stage.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/publishing-tools/staging/configuring-local-live-staging/images/05.png)

    WARNING

    When applications are checked, their data is copied to the staging environment, and it may not be possible to edit them directly in the live environment. When unchecking an application, first make sure that any changes in staging are published, since they may be lost. See [Managing Data and Content Types in Staging](https://learn.liferay.com/web/guest/w/dxp/site-building/publishing-tools/staging/managing-data-and-content-types-in-staging) for more information.
5.  Click on _Save_ to initiate the cloning process. The duration of this process depends on the size of your site.

    TIP

    Stage your site early on to reduce cloning time and record a more complete history of your site’s update history, since updates are only recorded once you enable page versioning.

Once the process is complete, you are ready to use Local Live staging. See [Site Staging UI Reference](https://learn.liferay.com/web/guest/w/dxp/site-building/publishing-tools/staging/site-staging-ui-reference) for information about navigating the staging environment’s publishing features.

### [Setting Up Staging for Asset Libraries](https://learn.liferay.com/web/guest/w/dxp/site-building/publishing-tools/staging/configuring-local-live-staging#setting-up-staging-for-asset-libraries) <a href="#setting-up-staging-for-asset-libraries" id="setting-up-staging-for-asset-libraries"></a>

1. Open the _Global Menu_ (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)) and click _Asset Libraries_ in the Applications tab.
2. Open the desired asset library and click _Staging_ under Publishing.
3.  Select _Local Live_.

    ![Select Local Live.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/publishing-tools/staging/configuring-local-live-staging/images/06.png)
4.  Select the application data you want to stage.

    For Asset Libraries, you can stage data for _Documents and Media_ and _Web Content_.

    ![Select the application data you want to stage.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/publishing-tools/staging/configuring-local-live-staging/images/07.png)
5.  Click _Save_ to initiate the cloning process. The duration of this process depends on the size of the asset library.

    Once the process is complete, you are ready to use Local Live staging in your asset library.

### [Disabling Local Live Staging](https://learn.liferay.com/web/guest/w/dxp/site-building/publishing-tools/staging/configuring-local-live-staging#disabling-local-live-staging) <a href="#disabling-local-live-staging" id="disabling-local-live-staging"></a>

If for any reason you must disable staging for your site or asset library, you can do that from your staging environment. However, be aware that disabling Local Live staging deletes the staging environment, along with all unpublished content. For this reason, ensure all necessary information is published or preserved elsewhere before disabling staging.

TIP

This process may take some time, depending on the size of your staging environment. It’s best not to disable staging when your Liferay instance is busy.

Follow these steps to disable Local Live staging:

1. Open the Staging application in your site or asset library.
2.  Click _Actions_ ( ![Actions button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png) ) in the Application bar and select _Staging Configuration_.

    ![Click the Actions button in the Application bar, and select Staging Configuration](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/publishing-tools/staging/configuring-local-live-staging/images/08.png)
3. Select _None_ for your configuration and click _Save_.

IMPORTANT

For most content types, staging only publishes the latest approved version of each content item. For this reason, disabling Local Live staging results in the loss of older content versions (e.g., documents). Once staging is disabled, only the latest published version in the live environment is preserved. Web content is the only exception.

\
