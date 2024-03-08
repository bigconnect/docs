# Using Staging in Asset Libraries

Liferay’s Staging application is a publishing tool for managing changes on DXP. For Asset Libraries, Staging provides a working environment for adding, removing, and editing assets in the Web Content or Documents and Media applications before making those changes live. You can access the Staging application for an Asset Library via its menu page.

![Access Staging via an Asset Library's menu page.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/publishing-tools/staging/using-staging-in-asset-libraries/images/01.png)

From here, you can enable [Local Live](https://learn.liferay.com/web/guest/w/dxp/site-building/publishing-tools/staging/configuring-local-live-staging) or [Remote Live](https://learn.liferay.com/web/guest/w/dxp/site-building/publishing-tools/staging/configuring-remote-live-staging) Staging for individual Asset Libraries following the same process as you would for a Site. The only difference is that staged content options are limited to the applications enabled for the Asset Library.

WARNING

Asset Libraries with Staging enabled can only be connected to Sites that also have Staging enabled. When Staging is enabled for an Asset Library, existing Site connections are automatically removed if the Site doesn’t also have Staging enabled.

Sites with Staging enabled can be connected to Asset Libraries without Staging enabled. However, this is **not** recommended, since it can cause duplicate Site links and prevent the removal of Asset Library connections if Staging is ever enabled for the Asset Library. For this reason, you should only connect staged Sites with staged Asset Libraries.

Once Staging is enabled for an Asset Library, you can toggle between Live and Staging environments via the Staging bar at the top of the page. While in the Staging environment, you can also access _Publish to Live_ to initiate a new publishing process.

![Navigate to the Asset Library to access the Staging bar.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/publishing-tools/staging/using-staging-in-asset-libraries/images/02.png)

Many editing options can only be accessed in the Staging environment. This is to maintain a clear development process and avoid data conflicts due to edits in the Live environment.

When changes are made in the Staging environment, they can then be published all at once, or one at a time. As with [Sites](https://learn.liferay.com/web/guest/w/dxp/site-building/publishing-tools/staging/site-staging-ui-reference#publish-to-live), you can use [simple](https://learn.liferay.com/web/guest/w/dxp/site-building/publishing-tools/staging/site-staging-ui-reference#simple-publishing) or [advanced](https://learn.liferay.com/web/guest/w/dxp/site-building/publishing-tools/staging/site-staging-ui-reference#advanced-publishing) publishing options.

\
