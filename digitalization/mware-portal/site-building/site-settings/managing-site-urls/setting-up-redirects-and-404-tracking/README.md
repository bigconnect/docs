# Setting Up Redirects and 404 Tracking

Maintaining URL integrity is important for any site. Liferay’s _Redirection_ tool provides a convenient and integrated way to manage HTTP redirects and track 404 errors for individual sites. Use it to avoid and fix broken links, improving both user experience and site SEO.

To access the application, open the _Site Menu_ (![Site Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu.png)) and go to _Configuration_ → _Redirection_.

![Go to the Redirection page in the Site Menu.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-settings/managing-site-urls/setting-up-redirects-and-404-tracking/images/01.png)

Liferay automatically creates a friendly URL history for page, blog entry, and document URLs. See [Configuring Friendly URL Redirects](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/managing-site-urls/configuring-friendly-url-redirects) for more information.

### [Defining Redirects](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/managing-site-urls/setting-up-redirects-and-404-tracking#defining-redirects) <a href="#defining-redirects" id="defining-redirects"></a>

You can define two kinds of redirects:

**Alias Redirects**: Define permanent (301) or temporary (302) URL redirects using an absolute source and destination URL.

**Pattern Redirects** 7.4 U48+/GA48+: Define bulk redirects using regular expressions that set patterns for source URLs and their replacements.

Liferay recommends alias redirects in most cases because they are more predictable. Pattern redirects are best for matching families of URLs. Depending on the exact regular expression, pattern redirects are slower and result in more URL matches than intended. See [Using Alias Redirects](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/managing-site-urls/setting-up-redirects-and-404-tracking/using-alias-redirects) and [Using Pattern Redirects](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/managing-site-urls/setting-up-redirects-and-404-tracking/using-pattern-redirects) for more information.

### [Resolving 404 Errors](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/managing-site-urls/setting-up-redirects-and-404-tracking#resolving-404-errors) <a href="#resolving-404-errors" id="resolving-404-errors"></a>

In the Redirection application, you can view and manage site requests that result in a 404 error, so you can assess and quickly resolve any issues. For Liferay 7.4+, 404 tracking is disabled by default. Enabling this feature activates the 404 URLs tab in the Redirection application and begins cataloging errors. See [Tracking 404 URLs](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/managing-site-urls/setting-up-redirects-and-404-tracking/tracking-404-urls) for more information.
