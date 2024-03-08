# Configuring Page Sets

When Private Pages are enabled, Site Pages are divided into public and private [Page Sets](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/understanding-pages/understanding-pages#page-sets). These sets group Pages so you can apply bulk configurations to each type of Page. Settings configured for a Page Set apply to all grouped pages and override configurations at the Site level. Page Set configurations are overridden by [customizations to individual pages](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-settings/configuring-individual-pages).

If Private Pages are disabled for your Liferay System, follow these steps to access bulk configuration Options for your Pages:

1. Open the _Site Menu_ (![Site Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu.png)), expand _Site Builder_, and click _Pages_.
2. Click the _Actions_ button (![Actions Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) in the Application Bar and select _Configuration_.

![Click the Actions button in the Application Bar and select Configuration](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-settings/configuring-page-sets/images/01.png)

If Private Pages are enabled for your Liferay system, follow these steps to access Page Set configuration options:

1. Open the _Site Menu_ (![Site Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu.png)), expand _Site Builder_, and click _Pages_.
2. Click the _Gear_ icon (![Gear Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-cog3.png)) for either _Public Pages_ or _Private Pages_.

![Click the Gear icon to access the Page Set's configuration Options.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-settings/configuring-page-sets/images/02.png)

### [Configuring the Page Set’s Look and Feel](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-settings/configuring-page-sets#configuring-the-page-sets-look-and-feel) <a href="#configuring-the-page-sets-look-and-feel" id="configuring-the-page-sets-look-and-feel"></a>

In the _Look and Feel_ tab, you can customize the Page Set’s theme and logo settings.

#### [Look and Feel](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-settings/configuring-page-sets#look-and-feel) <a href="#look-and-feel" id="look-and-feel"></a>

Themes create the overall feel for the Site and can transform the look entirely. The _Current Theme_ section displays the Theme currently applied to the Page Set, along with any configurable theme settings and color schemes that the Theme has. Many Themes include more than one color scheme, which keeps the existing look and feel while giving the Site a different flavor.

![The Look and Feel interface allows you to choose a theme for the current site.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-settings/configuring-page-sets/images/03.png)

To change the current theme,

1. Click the _Change Current Theme_ button and select the Theme from the window that appears.
2.  Select your desired theme.

    ![Choose from one of the available themes.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-settings/configuring-page-sets/images/04.png)
3. Click _Save_ to apply the new Theme to the Page Set.

You can enter custom CSS in the _CSS_ section for modifying your theme. You can apply Themes to the entire Site (described here) or to individual pages (described in [Configuring Pages](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-settings/configuring-individual-pages#look-and-feel)).

#### [Logo](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-settings/configuring-page-sets#logo) <a href="#logo" id="logo"></a>

By default, the Liferay logo is used for your Site’s pages. To use your own logo for a Site,

1. Expand the _Logo_ section.
2. Click the _Change_ button.
3. Browse to the location of your logo. Make sure your logo fits the space in the top left corner of the Theme you’re using for your Site. If you don’t, your other page elements are pushed aside to make way for the logo.
4.  Choose whether to display the Site name on the Site. When _Show Site Name_ is enabled, the Site name appears next to the logo.

    NOTE

    This option is enabled by default and can’t be disabled if the _Allow Site Administrators to set their own logo_ option is disabled in _Instance Settings_. Removing the Site name is not available for the default Liferay Site—you can configure this only for new Sites and User pages.
5. Click _Save_.

The Site’s logo settings are now configured.

### [Configuring the Page Set’s Advanced Settings](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-settings/configuring-page-sets#configuring-the-page-sets-advanced-settings) <a href="#configuring-the-page-sets-advanced-settings" id="configuring-the-page-sets-advanced-settings"></a>

The _Advanced_ tab contains several options that can impact the Site and overall performance. Administrators should proceed with caution.

![The Advanced tab contains multiple options that enhances your Site.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-settings/configuring-page-sets/images/05.png)

#### [JavaScript](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-settings/configuring-page-sets#javascript) <a href="#javascript" id="javascript"></a>

In the _JavaScript_ tab, you can paste JavaScript in the JavaScript editor. Code entered here is executed at the bottom of every page in the Site. Your Site’s JavaScript is most likely (and should be) included with the Theme. However, this may be a good place to quickly test JavaScript code while not in production.

#### [Advanced Settings](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-settings/configuring-page-sets#advanced-settings) <a href="#advanced-settings" id="advanced-settings"></a>

If you have multiple Sites, one Site is marked as the _Default Site_ that visitors see when they visit your domain name. By default, only the default Site’s Public Pages are displayed in the navigation.

You can display another Site’s Public Pages in the default Site’s navigation by enabling the _Merge public pages_ option for that Site.

WARNING

Adding too many pages to the main navigation can make it become unwieldy very quickly.

1.  Check the _Merge Liferay DXP public pages_ box.

    ![You can merge your Site's top level with the DXP Guest site.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-settings/configuring-page-sets/images/06.png)
2. Click _Save_ when finished.

#### [Mobile Device Rules](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-settings/configuring-page-sets#mobile-device-rules) <a href="#mobile-device-rules" id="mobile-device-rules"></a>

You can add _Mobile Device Rules_ to configure behaviors for specific mobile devices or types for the Page Set. Mobile device rules are inherited from your Public Pages, but you can define specific rules per page. You can edit the Look and Feel of specific pages for mobile devices, including the theme. See [Mobile Device Rules](https://learn.liferay.com/web/guest/w/dxp/site-building/optimizing-sites/building-a-responsive-site/creating-mobile-device-rules) for more information.

#### [Robots](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-settings/configuring-page-sets#robots) <a href="#robots" id="robots"></a>

Here you can configure search and indexing rules in the `robots.txt` rules for the domain’s public and private pages. The `robots.txt` file provides instructions to search engines and other tools that are automatically crawling and indexing your Site. For example, you can specify not to index certain pages.

#### [Sitemap](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-settings/configuring-page-sets#sitemap) <a href="#sitemap" id="sitemap"></a>

This generates a sitemap you can send to some search engines so they can crawl your Site. It uses the industry standard sitemap protocol. Select a search engine link to send the sitemap to it (only required once per Site), or select the _preview_ link to see the generated XML that is sent to search engines.

### [Using the Page Tree Menu](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-settings/configuring-page-sets#using-the-page-tree-menu) <a href="#using-the-page-tree-menu" id="using-the-page-tree-menu"></a>

In Liferay DXP 7.3, you can access the same settings from the _Page Tree_ menu:

1. Open the _Site Menu_ (![Site Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-menu.png)) and click _Page Tree_ (![Page Tree](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-page-tree.png)).
2. Select _Public Pages_ or _Private Pages_ from the dropdown menu.
3.  Click the Gear icon.

    ![You can access the same Page Sets options from the Page Tree menu.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-settings/configuring-page-sets/images/07.png)
