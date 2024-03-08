# Managing Site URLs

Liferay DXP provides multiple out-of-the-box features for configuring and managing site URLs.

### [Virtual Hosts Site URLs](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/managing-site-urls#virtual-hosts-site-urls) <a href="#virtual-hosts-site-urls" id="virtual-hosts-site-urls"></a>

Configure virtual hosts for your Liferay server to connect domain names (e.g., `www.helloworld.com`) and subdomain names (e.g. `developers.helloworld.com`) to your sites. In this way you can host multiple, separate sites on one Liferay DXP server. See [Configuring Virtual Hosts Site URLs](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/managing-site-urls/configuring-virtual-hosts-site-urls) for more information.

### [Friendly URLs](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/managing-site-urls#friendly-urls) <a href="#friendly-urls" id="friendly-urls"></a>

Define friendly URLs to help users quickly access your sites. Each friendly URL on your Liferay server must be unique and is appended to `https://[server:port]/web` (e.g., `https://localhost:8080/web/hello-world/`). See [Configuring Your Site’s Friendly URL](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/managing-site-urls/configuring-your-sites-friendly-url) for more information.

NOTE

You can also configure friendly URLs for pages and documents. See [Configuring Your Page’s Friendly URL](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-settings/configuring-your-pages-friendly-url) and [Configuring Document URLs](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/documents-and-media/uploading-and-managing/configuring-document-urls) for more information.

### [Site Redirects](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/managing-site-urls#site-redirects) <a href="#site-redirects" id="site-redirects"></a>

Set up URL redirects for your sites to avoid 404s and improve user experience. With Liferay DXP, you can define two types of redirects: alias redirects and pattern redirects.

**Alias Redirects**: These redirects explicitly define an absolute source URL and an absolute destination URL. For this reason, they are more predictable and recommended in most use cases. See [Using Alias Redirects](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/managing-site-urls/setting-up-redirects-and-404-tracking/using-alias-redirects) for instructions on setting up alias redirects for your site.

**Pattern Redirects** 7.4 U42+/GA42+: These redirects use regular expressions to define patterns for source URLs and their replacements. For this reason, they are best used to match families of URLs quickly. However, depending on the regular expression, they can be slower than alias redirects and result in more URL matches than intended. See [Using Pattern Redirects](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/managing-site-urls/setting-up-redirects-and-404-tracking/using-pattern-redirects) for more information on setting them up for your site.

### [404 URLs](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/managing-site-urls#404-urls) <a href="#id-404-urls" id="id-404-urls"></a>

Use the Redirection tool to track and manage Site requests leading to 404 errors. When enabled, Liferay DXP catalogs each 404 error, so you can assess and quickly resolve any issues. See [Tracking 404 URLs](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/managing-site-urls/setting-up-redirects-and-404-tracking/tracking-404-urls) for more information and available configuration options.
