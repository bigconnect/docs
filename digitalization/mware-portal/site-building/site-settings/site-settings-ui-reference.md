# Site Settings UI Reference

Starting with Liferay DXP 7.4, the Site Settings application layout is consistent with other configuration areas, such as Instance Settings or System Settings.

NOTE

The Site Settings application changes significantly in Liferay DXP 7.4+. See [Site Settings UI Reference in Liferay DXP 7.2 and 7.3](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/site-settings-ui-reference#site-setting-ui-reference-in-liferay-dxp-7-2-and-7-3) below for information on previous versions.

### [Accessing the Site Settings Application](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/site-settings-ui-reference#accessing-the-site-settings-application) <a href="#accessing-the-site-settings-application" id="accessing-the-site-settings-application"></a>

1. Go to Site Administration → _Configuration_ → _Site Settings_.
2. Click on a setting to open its configuration.

You can find the following configuration areas:

* [Commerce](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/site-settings-ui-reference#commerce)
* [Platform](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/site-settings-ui-reference#platform)
* [Content and Data](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/site-settings-ui-reference#content-and-data)
* [Other](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/site-settings-ui-reference#other)

![Site Settings are organized into four areas.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-settings/site-settings-ui-reference/images/20.png)

### [Commerce](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/site-settings-ui-reference#commerce) <a href="#commerce" id="commerce"></a>

By default, the Commerce area includes the following settings:

* Catalog
* Orders
* Payment
* Shipping

Here you can configure the Commerce settings at the Site scope. For more information about Commerce settings, see [Liferay Commerce Configuration Overview](https://learn.liferay.com/commerce/latest/en/store-management/liferay-commerce-configuration-overview.html).

![Default Site Settings for Commerce](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-settings/site-settings-ui-reference/images/21.png)

### [Platform](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/site-settings-ui-reference#platform) <a href="#platform" id="platform"></a>

By default, the Platform area includes the following settings:

* [Analytics](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/site-settings-ui-reference#analytics)
* [Localization](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/site-settings-ui-reference#localization)
* [Site Configuration](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/site-settings-ui-reference#site-configuration)
* [Third Party](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/site-settings-ui-reference#third-party)
* [Third Party Applications](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/site-settings-ui-reference#third-party-applications)
* [Users](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/site-settings-ui-reference#users)

#### [Analytics](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/site-settings-ui-reference#analytics) <a href="#analytics" id="analytics"></a>

Use these settings to configure the Google Analytics options for your Site. You can also set the Piwik Analytics tracking code.

NOTE

In the Piwik configuration, enter the full script code including the script start and end tags.

If you require a different analytics service, you can add it. See [Adding a New Analytics Service](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/adding-a-new-analytics-service) for more information and how to add additional fields for different analytics services.

#### [Localization](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/site-settings-ui-reference#localization) <a href="#localization" id="localization"></a>

Here you can choose whether to use the installation’s default language options or define your own for the Site. For more information on setting your Site’s language options, read [Site Localization](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/site-localization).

![Use the Localization settings to set your Site language options.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-settings/site-settings-ui-reference/images/22.png)

#### [Site Configuration](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/site-settings-ui-reference#site-configuration) <a href="#site-configuration" id="site-configuration"></a>

By default, Site Configuration includes the Details and Site URL sections.

[**Details**](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/site-settings-ui-reference#details)

| Setting                            | Description                                                                                                                                                                                                                                               |
| ---------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Site ID                            | A unique number automatically generated for a Site at its creation. This ID is permanent and cannot be changed.                                                                                                                                           |
| Description                        | Use the this field to explain a Site’s purpose.                                                                                                                                                                                                           |
| Parent Site                        | Designate a Site as a child Site by selecting its parent Site. See [Site Hierarchies](https://learn.liferay.com/web/guest/w/dxp/site-building/sites/site-hierarchies) for more information.                                                               |
| Membership Type                    | Determines how restrictive a Site’s membership is. See [Changing Site Membership Types](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/site-users/changing-site-membership-type) for more information.                             |
| Allow Manual Membership Management | Determine whether to allow members to be manually added or removed from a Site. See [Changing Site Membership Type](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/site-users/changing-site-membership-type) for more information. |

[**Site URL**](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/site-settings-ui-reference#site-url)

| Setting      | Description                                                                                                                                                                                                                                                    |
| ------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Friendly URL | Set a custom URL for Site pages. See [Configuring Your Site’s Friendly URL](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/managing-site-urls/configuring-your-sites-friendly-url) for more information.                                |
| Pages        | Set Virtual Hosts that map to friendly URL for the Site’s pages. See [Configuring Virtual Hosts Site URLs](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/managing-site-urls/configuring-virtual-hosts-site-urls) for more information. |

#### [Third Party](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/site-settings-ui-reference#third-party) <a href="#third-party" id="third-party"></a>

Configure the PunchOut2Go Site setting for Liferay Commerce. For more information, see [Liferay Commerce Connector to PunchOut2Go Reference Guide](https://learn.liferay.com/commerce/latest/en/add-ons-and-connectors/liferay-commerce-connector-to-punchout2go-reference-guide.html).

#### [Third Party Applications](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/site-settings-ui-reference#third-party-applications) <a href="#third-party-applications" id="third-party-applications"></a>

By default, Third Party Application includes the Maps and Google Places sections.

| Section            | Description                                                                                                                                                                                                                                                                                                                                                              |
| ------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Asset Auto Tagging | When enabled, assets are auto tagged by the providers configured at the Instance scope. See [Configuring Asset Auto Tagging](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/tags-and-categories/auto-tagging/configuring-asset-auto-tagging) for more information.                                                                           |
| Categorization     | Use this section to categorize your Site. This setting does not affect the Categories and Tags available for the Site. For more information on using Categories and Tags, see [Organizing Content with Categories and Tags](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/tags-and-categories/organizing-content-with-categories-and-tags). |

#### [Users](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/site-settings-ui-reference#users) <a href="#users" id="users"></a>

By default, the Users section includes the Default User Associations setting, where you can configure the default Roles and Teams for new Site members. For more information, see [Configuring Role and Team Defaults for Site Members](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/site-users/configuring-role-and-team-defaults-for-site-members).

### [Content and Data](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/site-settings-ui-reference#content-and-data) <a href="#content-and-data" id="content-and-data"></a>

By default, the Content and Data area includes the following settings:

* [Assets](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/site-settings-ui-reference#assets)
* [Click to Chat](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/site-settings-ui-reference#click-to-chat)
* [Community Tools](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/site-settings-ui-reference#community-tools)
* [Digital Signature](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/site-settings-ui-reference#digital-signature)
* [Documents and Media](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/site-settings-ui-reference#documents-and-media)
* [Message Boards](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/site-settings-ui-reference#message-boards)
* [Pages](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/site-settings-ui-reference#pages)
* [Recycle Bin](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/site-settings-ui-reference#recycle-bin)
* [Reports](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/site-settings-ui-reference#reports)
* [Sharing](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/site-settings-ui-reference#sharing)

#### [Assets](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/site-settings-ui-reference#assets) <a href="#assets" id="assets"></a>

| Section               | Description                                                                                                                                                                                                                                                                                                                                                   |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Maps                  | Select which maps API provider to use when displaying geo-localized assets in your Site. Geo-localized assets can be displayed for documents, web content articles, DDL records, and more. See [Geo-locating Assets](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/tags-and-categories/geolocating-assets) for more information. |
| Google Places API Key | Set the Google Places API Key.                                                                                                                                                                                                                                                                                                                                |

#### [Click to Chat](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/site-settings-ui-reference#click-to-chat) <a href="#click-to-chat" id="click-to-chat"></a>

> Available: Liferay 7.4 GA2+

You can integrate Liferay DXP with a number of live support chat platforms. The configuration at Site scope depends on the Click to Chat configuration at Instance scope. For more information, read [Enabling Automated Live Chat Systems](https://learn.liferay.com/web/guest/w/dxp/site-building/personalizing-site-experience/enabling-automated-live-chat-systems).

You can see the type of configuration for this setting at the Instance scope under the Site Settings Strategy text:

![Find the type of configuration at the Instance scope under Site Settings Strategy.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-settings/site-settings-ui-reference/images/23.png)

NOTE

Depending on your configuration at the Instance scope, you may not be able to update the configuration at Site scope. To verify your Instance Configuation, got to the Site Menu → Control Panel → Instance Settings → Content and Data → Click to Chat.

#### [Community Tools](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/site-settings-ui-reference#community-tools) <a href="#community-tools" id="community-tools"></a>

| Section  | Description                                                                                                                                                                                                                                                                       |
| -------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Mentions | Determines whether to allow Users to mention other Users in Site applications. See [Configuring Mentions](https://learn.liferay.com/web/guest/w/dxp/collaboration-and-social/notifications-and-requests/user-guide/configuring-mentions) for more information.                    |
| Ratings  | Here you can select the type of ratings used for the different Site applications. See [Configuring Content Rating Types](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/site-content-configurations/configuring-content-rating-type) for more information. |

#### [Digital Signature](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/site-settings-ui-reference#digital-signature) <a href="#digital-signature" id="digital-signature"></a>

You can integrate Liferay DXP with DocuSign to manage electronic signatures. The configuration at Site scope depends on the Digital Signature configuration at Instance scope. You can check the Instance configuration under the Site Settings Strategy text:

![Find the type of configuration at the Instance scope under Site Settings Strategy.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-settings/site-settings-ui-reference/images/24.png)

NOTE

Depending on your Digital Signature configuration at the Instance scope, you may not be able to update the configuration at Site scope. To verify your Instance Configuation, go to Site Menu → Control Panel → Instance Settings → Content and Data → Digital Signature.

#### [Documents and Media](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/site-settings-ui-reference#documents-and-media) <a href="#documents-and-media" id="documents-and-media"></a>

When enabled, a User with view permission can browse the Site’s document library files and folders. For more information, see [Securing Site Documents Content](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/site-content-configurations/securing-site-documents-content).

#### [Message Boards](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/site-settings-ui-reference#message-boards) <a href="#message-boards" id="message-boards"></a>

Using this option, you can enable automatic moderation of user messages on Message Boards after a predefined number of messages.

![Configure automatic moderation of Message Boards.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-settings/site-settings-ui-reference/images/25.png)

#### [Pages](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/site-settings-ui-reference#pages) <a href="#pages" id="pages"></a>

| Section                                      | Description                                                                                                                                                                                                                                                                                                                                                                                                                                |
| -------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Google PageSpeed Insights (Liferay DXP 7.4+) | Enable the Google PageSpeed Insight integration in Liferay DXP. Using this option, you can audit your pages for accessibility issues and check suggestions for fixing them. See [Analyzing SEO and Accessibility on Pages](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/page-performance-and-accessibility/analyze-seo-and-accessibility-on-pages) for more information.                                     |
| Open Graph                                   | These tags define page metadata to create engaging representations of your Site’s content when shared in applications that support the Open Graph protocol, such as Facebook, Slack, and Twitter. When enabled Open Graph `<meta>` tags are embedded in the `head` of the Site’s pages. For more information, read [Configuring Open Graph](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/configuring-open-graph). |
| Pages                                        | View the Site’s pages. On this section, you can also configure the propagation of changes from a Site template, if one exists. See [Site Templates](https://learn.liferay.com/web/guest/w/dxp/site-building/sites/site-templates) and [Propagating Template Changes](https://learn.liferay.com/web/guest/w/dxp/site-building/sites/site-templates/propagating-template-changes) for more information.                                      |

#### [Recycle Bin](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/site-settings-ui-reference#recycle-bin) <a href="#recycle-bin" id="recycle-bin"></a>

Configure the Recycle Bin for a Site. You can specify the number of minutes an asset remains in the Recycle Bin before automatic deletion. By default, the max age for recycled items is 43200 minutes (30 days). See [Configuring the Asset Recycle Bin for Sites](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/site-content-configurations/configuring-the-asset-recycle-bin-for-sites) for more information.

#### [Reports](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/site-settings-ui-reference#reports) <a href="#reports" id="reports"></a>

Settings for the Reports Group Service functionality.

#### [Sharing](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/site-settings-ui-reference#sharing) <a href="#sharing" id="sharing"></a>

| Section         | Description                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| --------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Content Sharing | Enable this option to allow child Sites to display content from this Site, including structures, templates, categories, or widget templates. When you disable this option, you immediately revoke content sharing from all child Sites. See [Managing Content Sharing Across Sites](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/site-content-configurations/managing-content-sharing-across-sites) for more information. |
| Sharing         | When enabled, Users can share items with one another. See [Enabling and Configuring Sharing](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/documents-and-media/publishing-and-sharing/managing-document-access/enabling-and-configuring-sharing) for more information.                                                                                                                                                |

### [Other](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/site-settings-ui-reference#other) <a href="#other" id="other"></a>

By default, the Other area includes the following settings:

| Section           | Description                                                                                                                                                                                                                                                                            |
| ----------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Other             | Includes custom Site settings you have defined for your Site, if any. For more information, read [Setting and Accessing Configurations](https://learn.liferay.com/web/guest/w/dxp/building-applications/core-frameworks/configuration-framework/setting-and-accessing-configurations). |
| Tax               | Configure the settings for the Remote Commerce Tax Configuration in Liferay Commerce. For more information, see the [Liferay Commerce](https://learn.liferay.com/commerce/latest/en/index.html) documentation.                                                                         |
| category.tax-rate | Configure the settings for the Liferay Commerce Avalara integration. For more information, see the [Liferay Commerce](https://learn.liferay.com/commerce/latest/en/index.html) documentation.                                                                                          |

### [Site Setting UI Reference in Liferay DXP 7.2 and 7.3](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/site-settings-ui-reference#site-setting-ui-reference-in-liferay-dxp-72-and-73) <a href="#site-setting-ui-reference-in-liferay-dxp-72-and-73" id="site-setting-ui-reference-in-liferay-dxp-72-and-73"></a>

To view a Site’s settings, open the Site Menu (![Site Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu.png)) and go to _Configuration_ → _Settings_, where you’ll see the following tabs:

* [General](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/site-settings-ui-reference#general)
* [Social](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/site-settings-ui-reference#social)
* [Language](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/site-settings-ui-reference#language)
* [Advanced](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/site-settings-ui-reference#advanced)

![Site Settings are organized into four tabs.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-settings/site-settings-ui-reference/images/01.png)

NOTE

Many of these settings can be localized to provide translations based on a user’s locale. See [Introduction to Localization](https://help.liferay.com/hc/en-us/articles/360028746672-Introduction-to-Localization) and [Modifying Localizable Site Fields](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/site-localization#modifying-localizable-site-fields) for more information.

#### [General](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/site-settings-ui-reference#general) <a href="#general" id="general"></a>

General settings range from core configuration, like a Site’s Membership Type, to finer details like Documents and Media indexing options. The tab’s content is organized into the following subsections: Details, Pages, Categorization, Site URL, Documents and Media, Open Graph, Asset Auto Tagging, Sharing, and Custom Fields.

[**Details**](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/site-settings-ui-reference#details-1)

**Site ID**: A unique number automatically generated for a Site at its creation. This ID is permanent and cannot be changed.

**Name**: Set a Site’s title using the _Name_ field. This title is displayed in the browser’s title bar as well as the header for each Site page. You can also localize a Site’s name via the _Language Flag_ button.

**Description**: Use the _Description_ field to explain a Site’s purpose. You can also localize a Site’s description via the _Language Flag_ button.

**Active**: Determine whether a Site is _Active_ or _Inactive_. While inactive, a site is inaccessible to users, though it can be reactivated if desired.

**Membership Type**: A _Membership Type_ determines how restrictive a Site’s membership is. See [Changing Site Membership Types](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/site-users/changing-site-membership-type) for more information.

**Allow Manual Membership Management**: Determine whether to allow members to be manually added or removed from a Site. If your Site’s membership is handled automatically by a membership policy, you can disable this setting. See [Managing Membership Policies for Sites](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/site-users/changing-site-membership-type) for more information.

**Parent Site**: Designate a Site as a child Site by selecting its parent Site. See [Site Hierarchies](https://learn.liferay.com/web/guest/w/dxp/site-building/sites/site-hierarchies) for more information.

**Limit Membership to Parent Site Members**: Determine whether a child Site’s membership is limited to members of its parent Site. This option only appears for child Sites.

[**Pages**](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/site-settings-ui-reference#pages-1)

View a Site’s Public and Private Pages, if any exist, and enable or disable the propagation of changes from the selected Site template. If they don’t exist, a Site Templates selector appears for creating pages with a template.

![In the Pages section, you can view a Site's Public and Private Pages.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-settings/site-settings-ui-reference/images/03.png)

#### [Custom Fields](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/site-settings-ui-reference#custom-fields) <a href="#custom-fields" id="custom-fields"></a>

View and configure any custom fields you’ve defined for your pages. With these fields, you can set page metadata, such as author, date of creation, and geolocation. This section only appears once you’ve configured custom fields for your Site. See [Custom Fields](https://learn.liferay.com/web/guest/w/dxp/system-administration/configuring-liferay/adding-custom-fields) for more information.

[**Categorization**](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/site-settings-ui-reference#categorization)

Use Categories and Tags to categorize a Site’s content so users can more easily find it. For more information on using tags and categories, see [Organizing Content with Tags and Categories](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/tags-and-categories/organizing-content-with-categories-and-tags).

[**Site URL**](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/site-settings-ui-reference#site-url-1)

**Friendly URL**: Set a custom URL for both public and private Site pages. See [Configuring Your Site’s Friendly URL](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/managing-site-urls/configuring-your-sites-friendly-url) for more information.

**Public and Private Virtual Host**: Set public and private Virtual Hosts to map to a Site’s Public and Private Pages. See [Configuring Virtual Hosts Site URLs](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/managing-site-urls/configuring-virtual-hosts-site-urls) for more information.

![In the Site URL section, you can configure your Site's Friendly URL, as well as Virtual Hosts for your Public and Private Pages.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-settings/site-settings-ui-reference/images/05.png)

[**Documents and Media**](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/site-settings-ui-reference#documents-and-media-1)

Determine whether to enable Directory Indexing for a Site. When enabled, a User with view permission can browse the Site’s document library files and folders.

[**Site Template**](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/site-settings-ui-reference#site-template)

If you created your Site using a _Site Template_, it’s displayed here, along with whether the template allows Users to modify pages that inherit it. See [Site Templates](https://learn.liferay.com/web/guest/w/dxp/site-building/sites/site-templates) for more information.

![View your page's selected template.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-settings/site-settings-ui-reference/images/07.png)

[**Open Graph**](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/site-settings-ui-reference#open-graph)

**Enable Open Graph**: Determine whether to embed [Open Graph](https://ogp.me/) `<meta>` tags in the `<head>` of your Site’s pages. These tags define page metadata to create engaging representations of your Site’s content when shared in applications that support the Open Graph protocol, such as Facebook, Slack, and Twitter.

**Image**: Use the Image field to define the following Open Graph `<meta>` properties for a page:

```html
   <meta property="og:image" content="http://example.com/ogp.jpg" />
   <meta property="og:image:secure_url" content="https://secure.example.com/ogp.jpg" />
   <meta property="og:image:type" content="image/jpeg" />
   <meta property="og:image:width" content="400" />
   <meta property="og:image:height" content="300" />
```

**Image Alt Description**: Use the Image Alt Description field to define the `og:image:alt` property for a page. You can also localize an image’s alt description via the _Language Flag_ button.

See [Configuring Open Graph](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/configuring-open-graph) to learn more.

![You can enable or disable Open Graph, define image meta tags, and see an image preview.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-settings/site-settings-ui-reference/images/08.png)

[**Sharing**](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/site-settings-ui-reference#sharing-1)

Determine whether to enable document sharing between Site Users. When enabled, Users can share items with one another. See [Sharing Documents with Other Users](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/documents-and-media/publishing-and-sharing/managing-document-access/sharing-documents-with-other-users) for more information.

[**Asset Auto Tagging**](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/site-settings-ui-reference#asset-auto-tagging)

Determine whether to enable asset auto tagging for a Site. When enabled, assets are auto tagged by the providers configured at Instance scope. See [Configuring Asset Auto Tagging](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/tags-and-categories/auto-tagging/configuring-asset-auto-tagging) for more information.

#### [Social](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/site-settings-ui-reference#social) <a href="#social" id="social"></a>

Here you can manage the social interactions between Users on a Site.

[**Ratings**](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/site-settings-ui-reference#ratings)

Here you can select the type of ratings used for the following Site applications: Comments, Knowledge Base, Blogs, Wiki, Message Boards, Web Content, and Documents and Media. See [Configuring Content Ratings Types](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/site-content-configurations/configuring-content-rating-type) for more information.

![You can select the type of ratings used for Site applications.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-settings/site-settings-ui-reference/images/11.png)

[**Mentions**](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/site-settings-ui-reference#mentions)

Determine whether to allow Users to mention other Users in Site applications. See [Mentioning Users](https://learn.liferay.com/web/guest/w/dxp/collaboration-and-social/notifications-and-requests/user-guide/configuring-mentions) for more information.

#### [Language](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/site-settings-ui-reference#language) <a href="#language" id="language"></a>

Here you can choose whether to use the installation’s default language options, or define your own for a Site.

![You can update language options through the Languages tab of Site Settings.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-settings/site-settings-ui-reference/images/13.png)

#### [Advanced](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/site-settings-ui-reference#advanced) <a href="#advanced" id="advanced"></a>

Here you can configure additional settings, including analytics services, content sharing, and more.

[**Default User Associations**](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/site-settings-ui-reference#default-user-associations)

Select the Roles and Teams that new Site members are assigned to by default. See [Configuring Role and Team Defaults for Site Members](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/site-users/configuring-role-and-team-defaults-for-site-members) for more information.

![You can select the Roles and Teams that new Site members are assigned to by default.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-settings/site-settings-ui-reference/images/14.png)

[**Analytics**](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/site-settings-ui-reference#analytics-1)

Use the provided fields to set a Site’s Google Analytics ID and configure additional Google Analytics options. You can also set the Piwik Analytics tracking code for a Site by entering the full script code, including script start and end tags.

If you require a different analytics service, you can add it. See [Adding a New Analytics Service](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/adding-a-new-analytics-service) for more information and how to add additional fields for different analytics services.

![In the Analytics section, you can configure a Site's analytic services.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-settings/site-settings-ui-reference/images/15.png)

[**Maps**](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/site-settings-ui-reference#maps)

Select which maps API provider to use when displaying geolocalized assets in your Site. Geolocalized assets can be displayed for documents, web content articles, DDL records, and more. See [Geolocating Assets](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/site-content-configurations/configuring-geolocation-for-assets) for more information.

![You can select the maps API provider used for Site geolocation.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-settings/site-settings-ui-reference/images/16.png)

[**Recycle Bin**](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/site-settings-ui-reference#recycle-bin-1)

Determine whether to enable the Recycle Bin for a Site. You can specify the number of minutes an asset remains in the Recycle Bin before being automatically deleted. By default, the max age for recycled items is 43200 minutes (i.e., 30 days). See [Configuring the Asset Recycle Bin for Sites](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/site-content-configurations/configuring-the-asset-recycle-bin-for-sites) for more information.

![You can configure a Site's Recycle Bin.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-settings/site-settings-ui-reference/images/17.png)

[**Content Sharing**](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/site-settings-ui-reference#content-sharing)

Determine whether child Sites can display content from this Site, including structures, templates, categories, widget templates, and more. Disabling this option immediately revokes content sharing from all child Sites. See [Managing Content Sharing Across Sites](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/site-content-configurations/managing-content-sharing-across-sites) for more information.

![You can determine whether child Sites can display content from this Site.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-settings/site-settings-ui-reference/images/18.png)

\
