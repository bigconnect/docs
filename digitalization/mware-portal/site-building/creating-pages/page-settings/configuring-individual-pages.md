# Configuring Individual Pages

You can configure individual pages to meet your needs. All changes made in this way are scoped to the selected page.

To configure a specific page, open the _Site Menu_ (![Site Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu.png)) and go to _Site Builder_ → _Pages_. Then click _Actions_ (![Actions button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-staging-bar-options.png)) for the page you want to customize and select _Configure_.

![Click the Actions button for the page you want to customize, and select Configure](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-settings/configuring-individual-pages/images/01.png)

Alternatively, you can access general page settings by clicking _Actions_ (![Actions button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-staging-bar-options.png)) → _Configuration_ (![Configuration button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-cog.png)) at the top right of the Application Bar.

These settings are organized into the following tabs:

* [General](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-settings/configuring-individual-pages#general)
* [Design](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-settings/configuring-individual-pages#design)
* [SEO](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-settings/configuring-individual-pages#seo)
* [Open Graph](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-settings/configuring-individual-pages#open-graph)
* [Custom Meta Tags](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-settings/configuring-individual-pages#custom-meta-tags)

NOTE

Many of these settings can be localized to provide translations based on a user’s locale. See [Initial Instance Localization](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/setting-up-liferay/initial-instance-localization) and [Modifying Localizable Site Fields](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/site-localization#modifying-localizable-site-fields) for more information.

### [General](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-settings/configuring-individual-pages#general) <a href="#general" id="general"></a>

Here you can configure the basic information of a page, including its name, friendly URL, layout, and more.

#### [Basic Info](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-settings/configuring-individual-pages#basic-info) <a href="#basic-info" id="basic-info"></a>

Use the name field to set a page’s title used for site navigation and displayed in a browser’s title bar. You can hide a page from menu display when it is configured to display the page hierarchy. When enabled, a hidden page still appears in other navigation menus if added to them. See [Managing Page Hierarchies](https://learn.liferay.com/web/guest/w/dxp/site-building/site-navigation/managing-page-hierarchies) for more information.

![Use the name field to set the page's title and check the checkbox to hide it from menu display.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-settings/configuring-individual-pages/images/02.png)

NOTE

Child pages inherit the parent page’s visibility. Also, hidden pages are not displayed on the default navigation menu, but can be displayed on any other created navigation menu.

#### [URL](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-settings/configuring-individual-pages#url) <a href="#url" id="url"></a>

Use the Friendly URL field to set a new URL for a page, or click the _Restore_ icon (![Friendly URL Restore icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-restore.png)) to see and restore a previously used URL. See [Configuring Your Page’s Friendly URL](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-settings/configuring-your-pages-friendly-url) for more information.

Use _Query String_ to set default parameters for a page when no other parameters are specified. Query strings offer significant advantages within web content templates. Additionally, you can control page link behavior by using the Target Type field, which has attributes like opening the link in a new tab or within a specific frame.

![Use the Friendly URL field to set a page's custom URL.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-settings/configuring-individual-pages/images/03.png)

TIP

While a page’s name and friendly URL are initially defined together at page creation, they are configured independently afterwards. It is best practice to update them together so that they match.

#### [Page Layout](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-settings/configuring-individual-pages#page-layout) <a href="#page-layout" id="page-layout"></a>

Layout templates dictate where widgets can be placed on a widget page. For basic widget pages created using either a global template or custom page template, the user can determine whether the page _Inherits Changes_ made to the chosen template. If the page does not inherit changes, select a custom layout to define row and column containers for its widgets.

![Select a layout template to define row and column containers for widgets.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-settings/configuring-individual-pages/images/04.png)

#### [Categorization](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-settings/configuring-individual-pages#categorization) <a href="#categorization" id="categorization"></a>

Use _Topics_ and _Tags_ to categorize a page’s content so users can more easily find it. For more information on using tags and categories, see [Organizing Content with Tags and Categories](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/tags-and-categories).

![Use Topics and Tags to categorize a page's content](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-settings/configuring-individual-pages/images/05.png)

#### [Customization Settings](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-settings/configuring-individual-pages#customization-settings) <a href="#customization-settings" id="customization-settings"></a>

Determine whether a widget page’s columns are _Customizable_ for users. If customization is enabled, specify the columns users can customize. See [Enabling User Personalization of Widget Pages](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/using-widget-pages/enabling-user-personalization-of-widget-pages) for more information.

![Determine whether users can customize a widget page.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-settings/configuring-individual-pages/images/06.png)

### [Design](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-settings/configuring-individual-pages#design) <a href="#design" id="design"></a>

Here you can configure the theme of your page and customize its design, CSS and JavaScript. You can also access this editor in a [page set’s configuration page](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-settings/configuring-page-sets).

#### [Theme](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-settings/configuring-individual-pages#theme) <a href="#theme" id="theme"></a>

Here you can use the page set’s theme settings or define a custom theme for your page. When you choose to customize the page’s theme, you can toggle various theme settings.

![Determine whether to use the page set's theme settings or define a custom theme for your page.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-settings/configuring-individual-pages/images/07.png)

#### [Basic Settings](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-settings/configuring-individual-pages#basic-settings) <a href="#basic-settings" id="basic-settings"></a>

Here you can quickly and easily change some areas of your page’s design system and styling.

**Favicon**: Change the default favicon applied from the theme.

**Master**: Change the master page template. See [Master Page Templates](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/defining-headers-and-footers/master-page-templates) for more information.

**Style Books**: Change the default style book applied from the theme. See [Using a Style Book to Standardize Site Appearance](https://learn.liferay.com/web/guest/w/dxp/site-building/site-appearance/style-books/using-a-style-book-to-standardize-site-appearance) for more information.

**Icon**: Choose an icon to display along with page’s name in the site’s navigation menus.

![Determine the favicon, master page template, style book and icon for your page.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-settings/configuring-individual-pages/images/08.png)

NOTE

Content pages also include Master and Style Book settings in the Edit sidebar menu under Page Design Options (![Page Design Options icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-format.png)).

#### [Customization](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-settings/configuring-individual-pages#customization) <a href="#customization" id="customization"></a>

You can also access a CSS and JavaScript editors where you can add custom code to the page.

**Theme CSS Client Extension:** Add [theme CSS client extensions](https://learn.liferay.com/web/guest/w/dxp/building-applications/client-extensions/front-end-client-extensions#theme-css-client-extensions) to fully replace the default CSS contained in the theme.

**CSS Client Extensions:** Add [CSS client extensions](https://learn.liferay.com/web/guest/w/dxp/building-applications/client-extensions/front-end-client-extensions#css-client-extensions) to extend the page’s CSS with client extensions. They are loaded after the theme CSS and after master extensions.

**Custom CSS:** Custom CSS can be applied and is loaded after the theme.

**JavaScript Client Extension and Custom JavaScript:** [JavaScript client extensions](https://learn.liferay.com/web/guest/w/dxp/building-applications/client-extensions/front-end-client-extensions#javascript-client-extensions) can be added to have JavaScript run when the page using this master page template is visited. Custom JavaScript code can also be added and is executed at the bottom of the page.

![Use CSS and JavaScript editors to add custom code to the page](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-settings/configuring-individual-pages/images/09.png)

TIP

If your site’s theme uses JavaScript, it’s best to add custom JavaScript code to the theme rather than an individual page or page set. This ensures your site’s JavaScript code remains in one place. If your theme does not use JavaScript, place all of your site’s JavaScript code here.

#### [Advanced Settings](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-settings/configuring-individual-pages#advanced-settings) <a href="#advanced-settings" id="advanced-settings"></a>

Access additional settings to customize a page’s content and features.

**Custom Fields**: View and configure the Custom Fields you’ve defined for site pages. See [Custom Fields](https://learn.liferay.com/web/guest/w/dxp/system-administration/configuring-liferay/adding-custom-fields) for more information.

**Embedded Widgets**: Access widgets you’ve embedded in a page using [Web Content Templates](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/web-content/web-content-templates/embedding-widgets-in-templates), [Page Fragments](https://learn.liferay.com/web/guest/w/dxp/site-building/developer-guide/developing-page-fragments/developing-fragments-intro), or [Themes](https://learn.liferay.com/web/guest/w/dxp/site-building/site-appearance/themes/introduction-to-themes).

**Mobile Device Rules**: Determine whether a page uses its page set’s [rules for mobile devices](https://learn.liferay.com/web/guest/w/dxp/site-building/optimizing-sites/building-a-responsive-site/creating-mobile-device-rules) or is uniquely configured. These rules determine how a page’s content renders for different types of mobile devices.

NOTE

The _Custom Fields_ and _Embedded Widget_ sections only appear once you’ve created custom fields and embedded widgets in a page.

### [SEO](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-settings/configuring-individual-pages#seo) <a href="#seo" id="seo"></a>

Search Engine Optimization (SEO) refers to the methods used to improve your page’s ranking in Search Engine Results Pages (SERP). The SEO tab provides easy access to settings you can use to optimize your page’s content. See [Configuring SEO and Open Graph](https://learn.liferay.com/web/guest/w/dxp/site-building/displaying-content/using-display-page-templates/configuring-seo-and-open-graph) for more information about the following settings.

#### [HTML Title](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-settings/configuring-individual-pages#html-title) <a href="#html-title" id="html-title"></a>

Use the HTML title field to define a page’s `<title>` tag. This title is used by search engines to rank your page and serves as the page’s heading in search engine results, though without replacing a page’s [name](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-settings/configuring-individual-pages#name-and-friendly-url). The recommended length for an HTML title is under 60 characters.

#### [Description](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-settings/configuring-individual-pages#description) <a href="#description" id="description"></a>

Use the description field to define a page’s `<description>` tag. This description is used by search engines to rank your page and also appears in search result previews of your page. The recommended length for a description is under 155 characters.

#### [Custom Canonical URL](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-settings/configuring-individual-pages#custom-canonical-url) <a href="#custom-canonical-url" id="custom-canonical-url"></a>

Set a custom canonical URL for a page. Custom URLs configured in this manner take precedence over the System Settings configuration.

#### [Keywords](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-settings/configuring-individual-pages#keywords) <a href="#keywords" id="keywords"></a>

List words that people are likely to use when searching for your page’s content. These keywords contribute to your page’s ranking.

TIP

Use keywords often throughout your page’s content, including titles, headings, descriptions, paragraphs, and image alt text.

![Set the page's HTML title, description, custom canonical URL and keywords to improve your page's ranking in Search Engine Results Pages (SERP)](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-settings/configuring-individual-pages/images/10.png)

#### [Preview](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-settings/configuring-individual-pages#preview) <a href="#preview" id="preview"></a>

Preview how a page is displayed in Search Engine Results Pages (SERP) based on your configuration of its HTML title, description, and custom URL.

![Preview how a page is displayed in search engine results.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-settings/configuring-individual-pages/images/11.png)

#### [Robots](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-settings/configuring-individual-pages#robots) <a href="#robots" id="robots"></a>

Configure `robots.txt` rules for a page. These rules provide instructions to search engines and other tools crawling and indexing your site, either blocking or granting a crawler access to a specified path.

![Configure robots.txt rules for your page to provide instructions to search engines](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-settings/configuring-individual-pages/images/12.png)

#### [Sitemap](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-settings/configuring-individual-pages#sitemap) <a href="#sitemap" id="sitemap"></a>

Determine whether a page is included in the sitemap used by site crawlers. By excluding a page, you instruct site crawlers to skip it when crawling and indexing your site.

#### [Page Priority](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-settings/configuring-individual-pages#page-priority) <a href="#page-priority" id="page-priority"></a>

Set a page’s priority from 0.0 to 1.0. This informs tools crawling and indexing your site how a page should be prioritized relative to other pages in your site.

#### [Change Frequency](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-settings/configuring-individual-pages#change-frequency) <a href="#change-frequency" id="change-frequency"></a>

Use the drop-down menu to inform tools crawling and indexing your site how frequently a page is updated.

![Determine if the page is included in the sitemap, and set its page priority and change frequency](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-settings/configuring-individual-pages/images/13.png)

### [Open Graph](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-settings/configuring-individual-pages#open-graph) <a href="#open-graph" id="open-graph"></a>

Available: Liferay DXP/Portal 7.3+

[Open Graph](https://ogp.me/) is an Internet protocol that standardizes previews of site content when shared in application contexts that support it, such as Twitter, Facebook, or LinkedIn. It does this by embedding structured data in page headers as `<meta>` tags, similar to [RDFa](https://en.wikipedia.org/wiki/RDFa).

In the _Open Graph_ tab, you can use Open Graph `<meta>` tags to define a page’s metadata and create engaging representations of your content. Values defined here override default values defined at the [site](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/configuring-open-graph) level.

NOTE

Open Graph `<meta>` tags are only included in page headers to unauthenticated users. They are not included when the user is logged in.

#### [Image](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-settings/configuring-individual-pages#image) <a href="#image" id="image"></a>

Use the image field to define the following Open Graph `<meta>` properties for a page:

```html
<meta property="og:image" content="http://example.com/ogp.jpg" />
<meta property="og:image:secure_url" content="https://secure.example.com/ogp.jpg" />
<meta property="og:image:type" content="image/jpeg" />
<meta property="og:image:width" content="400" />
<meta property="og:image:height" content="300" />
```

#### [Image Alt Description](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-settings/configuring-individual-pages#image-alt-description) <a href="#image-alt-description" id="image-alt-description"></a>

Use the image alt description field to define the `og:image:alt` property for a page. You can also localize an image’s alt description via the _Language Flag_ button.

#### [Custom Title and Description](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-settings/configuring-individual-pages#custom-title-and-description) <a href="#custom-title-and-description" id="custom-title-and-description"></a>

Use the custom title and description fields to define the `og:title` and `og:description` properties. These properties create custom text used for rich previews in place of a page’s HTML title and description. You can also localize these values via their _Language Flag_ buttons.

#### [Preview Open Graph Page Representation](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-settings/configuring-individual-pages#preview-open-graph-page-representation) <a href="#preview-open-graph-page-representation" id="preview-open-graph-page-representation"></a>

Preview your page’s Open Graph configuration. This section displays how your page’s content appears when its URL is shared in contexts supporting the protocol. Though an image’s ratio may change depending on where the URL is posted.

![Preview your Open Graph configuration.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-settings/configuring-individual-pages/images/14.png)

### [Custom Meta Tags](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-settings/configuring-individual-pages#custom-meta-tags) <a href="#custom-meta-tags" id="custom-meta-tags"></a>

Here you can add custom `<meta>` tags to the `<head>` of a page. These tags are only visible in a page’s HTML source code and are used to describe a page’s data and determine how that data is displayed in different contexts, such as search engine results and social media posts. Each tag is defined using property and content attributes.

![Add custom meta tags to a page's head using the Property and Content fields.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-settings/configuring-individual-pages/images/15.png)
