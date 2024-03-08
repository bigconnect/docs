# Using Alias Redirects

Alias redirects define absolute source and destination URLs explicitly. Because of their predictability, alias redirects are recommended for most use cases. You can make alias redirects permanent (301) or temporary (302). Both types direct users automatically to a specified destination, but they each have unique implications for your site. Consider which type is best for your situation.

**Permanent**: Use a 301 code to inform search engines and other tools accessing your site that the requested resource has been permanently relocated. Search engines replace the old URL with the new destination in search results, and browsers cache the new destination for an indeterminate amount of time. A 301 code should only be used when you intend to use the destination URL indefinitely. This improves SEO and helps avoid unintended caching.

**Temporary**: Use a 302 code to inform search engines and other tools accessing your site that the requested resource is temporarily moved. A 302 code is useful when you want to conduct A/B testing or redirect traffic while fixing a problem, without impacting your site ranking. Liferay uses 302 codes by default.

See [Optimizing Sites](https://learn.liferay.com/web/guest/w/dxp/site-building/optimizing-sites) to learn more about the implications of each redirect type for SEO. Also, see [A/B Testing](https://learn.liferay.com/web/guest/w/dxp/site-building/optimizing-sites/ab-testing/ab-testing) to learn about using 302 codes when testing.

NOTE

If you need to perform bulk redirects, see [Using Pattern Redirects](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/managing-site-urls/setting-up-redirects-and-404-tracking/using-pattern-redirects).

### [Creating Alias Redirects](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/managing-site-urls/setting-up-redirects-and-404-tracking/using-alias-redirects#creating-alias-redirects) <a href="#creating-alias-redirects" id="creating-alias-redirects"></a>

1.  Open the _Site Menu_ (![Site Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu.png)) and go to _Configuration_ → _Redirection_.

    ![Go to the Aliases tab in the Redirection application.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-settings/managing-site-urls/setting-up-redirects-and-404-tracking/using-alias-redirects/images/01.png)
2. In the Aliases tab, click _Add_ (![Add button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png)).
3.  Enter a unique relative URL into the _Source URL_ field to determine the link that triggers the redirect. The entered value is added to the base URL displayed above it.

    ![Enter a unique relative URL into the Source URL field.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-settings/managing-site-urls/setting-up-redirects-and-404-tracking/using-alias-redirects/images/02.png)
4.  Enter an absolute URL into the _Destination URL_ field to specify the link for your desired destination.

    This destination can be internal or external, but must always be entered as an absolute URL. You can also click the _Check URL_ button (![Check URL Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-shortcut.png)) to verify the URL.

    ![Enter an absolute URL into the Destination URL field.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-settings/managing-site-urls/setting-up-redirects-and-404-tracking/using-alias-redirects/images/03.png)
5.  Use the _Type_ drop-down menu to define the redirect as permanent (301) or temporary (302).

    ![Determine whether the redirect is permanent or temporary.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-settings/managing-site-urls/setting-up-redirects-and-404-tracking/using-alias-redirects/images/04.png)
6.  You can set an automatic _Expiration Date_ for the redirect. Leave this field empty to avoid expiration.

    ![Determine whether the redirect expires.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-settings/managing-site-urls/setting-up-redirects-and-404-tracking/using-alias-redirects/images/05.png)
7. When finished, click _Create_ to save the redirect.

Once saved, Liferay validates your configuration. If other redirects point to your specified source URL, Liferay notifies you and asks if you want to update the existing redirects to avoid a redirect chain. To help reduce conflicts, Liferay prevents users both from defining multiple alias redirects for the same source URL and from directly redirecting URLs to themselves.

![Liferay helps you avoid creating redirect chains.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-settings/managing-site-urls/setting-up-redirects-and-404-tracking/using-alias-redirects/images/06.png)

WARNING

Avoid creating circular redirects. These are easy to create when defining redirects for content friendly URLs (e.g. redirecting between different friendly URLs for the same content). Liferay cannot reliably detect these loops.

### [Editing and Removing Alias Redirects](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/managing-site-urls/setting-up-redirects-and-404-tracking/using-alias-redirects#editing-and-removing-alias-redirects) <a href="#editing-and-removing-alias-redirects" id="editing-and-removing-alias-redirects"></a>

Once you’ve added a URL redirect, edit or remove it at any time by clicking its Actions button (![Actions button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) and selecting Edit or Delete.

![You can edit or remove a redirect at any time.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-settings/managing-site-urls/setting-up-redirects-and-404-tracking/using-alias-redirects/images/07.png)

NOTE

Changes made to a redirect may not immediately display in browsers that have cached the old redirect configuration.

\
