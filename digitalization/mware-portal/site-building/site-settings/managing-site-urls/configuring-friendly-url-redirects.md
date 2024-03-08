# Configuring Friendly URL Redirects

When you update friendly URLs for pages, blogs, and documents, Liferay automatically stores the old URLs. As long as the old URL remains in the history, Liferay redirects users to the latest version of the page or asset. This is to improve your site’s SEO and provide a more optimal user experience.

By default friendly URL redirects are temporary (302), but you can configure them to be permanent (301) if needed. You can also determine whether Liferay displays a message when users are redirected to a different friendly URL.

To configure friendly URL redirects,

1.  Open the _Global Menu_ (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)) and go to _Control Panel_ → _System Settings_ → _Pages_ → _Friendly URL Redirection_.

    Alternatively, you can configure these settings under Instance Settings.

    ![Navigate to the Friendly URL Redirection settings.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-settings/managing-site-urls/configuring-friendly-url-redirects/images/01.png)
2. Enable or disable _Show Alternative Layout Friendly URL Message_ to determine if Liferay displays a message whenever users are redirected to a different friendly URL.
3.  Set the Redirection Type to _Temporary_ or _Permanent_.

    **Permanent**: Use a 301 code to inform search engines and other tools accessing your site that the requested resource has been permanently relocated. Search engines replace the old URL with the new destination in search results, and browsers cache the new destination for an indeterminate amount of time. A 301 code should be used only when you intend to use the destination URL indefinitely. This improves SEO and helps avoid unintended caching.

    **Temporary**: Use a 302 code to inform search engines and other tools accessing your site that the requested resource has temporarily moved. A 302 code is useful when you want to conduct A/B testing or redirect traffic while fixing a problem, without impacting your site ranking. Liferay uses 302 codes by default.

    IMPORTANT

    Setting redirects to temporary after making them permanent can have unintended consequences. In some cases, users who previously received a permanent redirect may not be able to reach the original URL again. For this reason, use permanent redirects with care.
4. Click _Save_.

\
