# Individual Page Settings

On the Configure page are four tabs: General, SEO, Look and Feel, and Advanced. Options selected here have no effect on the rest of the Site; just the page you’ve selected. Many of these options are the same as those that configure the complete page set, so you can view more details in the [Configuring Page Sets](configuring-page-sets.md) article.

Note that many of the options are localizable, so you can provide translations based on the user’s locale.

### General <a href="#general" id="general"></a>

The _General_ tab lets you configure the basic information and design for the page. You can change the _Name_, _Friendly URL_, and _Page Layout_.

#### NAME AND FRIENDLY URL <a href="#name-and-friendly-url" id="name-and-friendly-url"></a>

The _Name_ is the title that appears in the browser’s title bar, and how the page is identified in the navigation. The _Friendly URL_ defines the page’s link. It is a best practice to have the URL match the name of the Page, so these two should generally be updated together.

#### PAGE LAYOUT <a href="#page-layout" id="page-layout"></a>

For Widget Pages, you can select a Layout Template that defines droppable locations for widgets. Layout templates define a number of sections with columns and rows. Widgets added to a section expand (or contract) horizontally to fill the space and can be stacked vertically.

![Figure 1: Setting a layout template for your page.](https://liferay-support.zendesk.com/hc/article\_attachments/360030189792/page-select-layout.png)

### Categorization and SEO <a href="#categorization-and-seo" id="categorization-and-seo"></a>

Managing your page’s content drastically improves your page’s organization and user experience. The Site page’s configuration options offers some opportunities to organize page content.

#### CATEGORIZATION <a href="#categorization" id="categorization"></a>

The _Categorization_ tab shows the categorization options. These tools help administrators organize the page so users can find your page and its content through search and navigation.

#### SEO <a href="#seo" id="seo"></a>

_SEO_ provides several ways to optimize the data the page provides to an indexer that’s crawling the page. You can set the various meta tags for description, keywords and robots. There’s also a separate Robots section for telling robots how frequently the page is updated and how it should be prioritized. If the page is localized, you can select a box to generate canonical links by language. If you want to set some of these settings for the entire Site, you can specify them from the Sitemaps and Robots tabs of the Manage Site Settings dialog box (see below).

Each asset (web content article, blog entry, etc.) has a unique URL. From the search engine’s point of view, this makes your pages rank higher since any references to variations of a specific URL are considered references to the same page.

You can also configure the page to use a custom canonical URL. To do so, set the _Use Custom Canonical URL_ toggle to _YES_, then enter your desired canonical URL in the field that appears. You can define a custom canonical URL for each language. If there’s no value for a specific language, the canonical URL for that language is controlled by the global/instance-level setting.

![Figure 2: Enter the custom canonical URL that you want to use for the page.](https://liferay-support.zendesk.com/hc/article\_attachments/360040164051/canonical-url-page.png)

You can also configure canonical URLs at the global and instance levels.

**Note:** Any custom canonical URLs set for individual pages take precedent over the global and instance level settings.

**Global:** _Control Panel_ → _Configuration_ → _System Settings_ → _Pages_ → _SEO_

**Instance:** _Control Panel_ → _Configuration_ → _Instance Settings_ → _Pages_ → _SEO_

Navigate to the level (global/instance) on which you want to configure canonical URLs, then choose one of the following in the _Canonical URL_ menu:

**Use Default Language URL (default):** When a user visits a page in any supported language, the default language’s URL is used as the canonical URL.

**Use Localized URL:** The page’s localized URL is used as the canonical URL.

![Figure 3: You can also configure canonical URLs at the global and instance levels.](https://liferay-support.zendesk.com/hc/article\_attachments/360040164031/canonical-url-system.png)

### Look and Feel <a href="#look-and-feel" id="look-and-feel"></a>

_Look and Feel_ lets you set a page-specific theme. You can inherit what you already have configured for your Page Set’s theme, or you can define a theme per page.&#x20;

### Advanced Settings <a href="#advanced-settings" id="advanced-settings"></a>

_Advanced Settings_ contains options useful for specific cases. Some of these are the same as the options available at the Site or Page Set level, but _Custom Fields_, _Embedded Widgets_, and _Customization Settings_ are unique to the individual page configuration.

#### QUERY STRING <a href="#query-string" id="query-string"></a>

You can set a query string to provide parameters to the page. This can become useful to web content templates. You can set a target for the page so that it pops up in a particularly named window or appears in a frameset. And you can set an icon for the page that appears in the navigation menu.

#### CUSTOM FIELDS <a href="#custom-fields" id="custom-fields"></a>

_Custom Fields_ lets you edit the custom fields you already have configured for the _Page_ resource. If you don’t have any custom fields configured in your Site, this option doesn’t appear. In this case, navigate to the Control Panel → _Custom Fields_ located under the _Configuration_ tab. These are metadata about the page and can be anything you like, such as author or creation date.

#### EMBEDDED WIDGETS <a href="#embedded-widgets" id="embedded-widgets"></a>

This option only appears if you have embedded one or more widgets on the page.

Widgets can be embedded on a page via web content template or fragment. To learn more about this, see Adding Templates. You can embed a widget on a page layout or theme programmatically.

#### CUSTOMIZATION SETTINGS <a href="#customization-settings" id="customization-settings"></a>

This configuration option in the _Advanced_ tab lets you mark specific sections of the page you want users to be able to customize.&#x20;

#### JAVASCRIPT <a href="#javascript" id="javascript"></a>

This shows a JavaScript editor for code that’s executed at the bottom of your page. If your Site’s theme uses JavaScript (as is usually the case), it’s best to add custom JavaScript code to the theme instead. This way, all your Site’s JavaScript code remains in one place.

This configuration option is also available for Page Sets like Public Pages and Private Pages.&#x20;

#### MOBILE DEVICE RULES <a href="#mobile-device-rules" id="mobile-device-rules"></a>

Apply rules for how this page should render for various mobile devices here. Create them by navigating to Site Administration menu and selecting _Configuration_ → _Mobile Device Families_.
