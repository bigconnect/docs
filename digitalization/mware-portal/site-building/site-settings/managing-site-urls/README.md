# Managing Site URLs

MWARE Portal provides multiple out-of-the-box features for configuring and managing site URLs.

### Virtual Hosts Site URLs <a href="#virtual-hosts-site-urls" id="virtual-hosts-site-urls"></a>

Configure virtual hosts for your portal server to connect domain names (e.g., `www.helloworld.com`) and subdomain names (e.g. `developers.helloworld.com`) to your sites. In this way you can host multiple, separate sites on one portal server. See Configuring Virtual Hosts Site URLs for more information.

### Friendly URLs <a href="#friendly-urls" id="friendly-urls"></a>

Define friendly URLs to help users quickly access your sites. Each friendly URL on your portal server must be unique and is appended to `https://[server:port]/web` (e.g., `https://localhost:8080/web/hello-world/`). See Configuring Your Site’s Friendly URL for more information.

NOTE

You can also configure friendly URLs for pages and documents. See Configuring Your Page’s Friendly URL and Configuring Document URLs for more information.

### Site Redirects <a href="#site-redirects" id="site-redirects"></a>

Set up URL redirects for your sites to avoid 404s and improve user experience. You can define two types of redirects: alias redirects and pattern redirects.

**Alias Redirects**: These redirects explicitly define an absolute source URL and an absolute destination URL. For this reason, they are more predictable and recommended in most use cases. See Using Alias Redirects for instructions on setting up alias redirects for your site.

**Pattern Redirects** 7.4 U42+/GA42+: These redirects use regular expressions to define patterns for source URLs and their replacements. For this reason, they are best used to match families of URLs quickly. However, depending on the regular expression, they can be slower than alias redirects and result in more URL matches than intended. See Using Pattern Redirects for more information on setting them up for your site.

### 404 URLs <a href="#id-404-urls" id="id-404-urls"></a>

Use the Redirection tool to track and manage Site requests leading to 404 errors. When enabled, the portal catalogs each 404 error, so you can assess and quickly resolve any issues. See Tracking 404 URLs for more information and available configuration options.
