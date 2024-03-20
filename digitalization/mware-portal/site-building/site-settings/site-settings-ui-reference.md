# Site Settings UI Reference

Starting with version 7.4, the Site Settings application layout is consistent with other configuration areas, such as Instance Settings or System Settings.

NOTE

The Site Settings application changes significantly in version 7.4+. See Site Settings UI Reference in versions 7.2 and 7.3 below for information on previous versions.

### Accessing the Site Settings Application <a href="#accessing-the-site-settings-application" id="accessing-the-site-settings-application"></a>

1. Go to Site Administration → _Configuration_ → _Site Settings_.
2. Click on a setting to open its configuration.

You can find the following configuration areas:

* Commerce
* Platform
* Content and Data
* Other

![Site Settings are organized into four areas.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-settings/site-settings-ui-reference/images/20.png)

### Commerce <a href="#commerce" id="commerce"></a>

By default, the Commerce area includes the following settings:

* Catalog
* Orders
* Payment
* Shipping

Here you can configure the Commerce settings at the Site scope. For more information about Commerce settings, see Commerce Configuration Overview.

![Default Site Settings for Commerce](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-settings/site-settings-ui-reference/images/21.png)

### Platform <a href="#platform" id="platform"></a>

By default, the Platform area includes the following settings:

* Analytics
* Localization
* Site Configuration
* Third Party
* Third Party Applications
* Users

#### Analytics <a href="#analytics" id="analytics"></a>

Use these settings to configure the Google Analytics options for your Site. You can also set the Piwik Analytics tracking code.

NOTE

In the Piwik configuration, enter the full script code including the script start and end tags.

If you require a different analytics service, you can add it. See Adding a New Analytics Service for more information and how to add additional fields for different analytics services.

#### Localization <a href="#localization" id="localization"></a>

Here you can choose whether to use the installation’s default language options or define your own for the Site. For more information on setting your Site’s language options, read Site Localization.

![Use the Localization settings to set your Site language options.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-settings/site-settings-ui-reference/images/22.png)

#### Site Configuration <a href="#site-configuration" id="site-configuration"></a>

By default, Site Configuration includes the Details and Site URL sections.

**Details**

| Setting                            | Description                                                                                                                             |
| ---------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| Site ID                            | A unique number automatically generated for a Site at its creation. This ID is permanent and cannot be changed.                         |
| Description                        | Use the this field to explain a Site’s purpose.                                                                                         |
| Parent Site                        | Designate a Site as a child Site by selecting its parent Site. See Site Hierarchies for more information.                               |
| Membership Type                    | Determines how restrictive a Site’s membership is. See Changing Site Membership Types for more information.                             |
| Allow Manual Membership Management | Determine whether to allow members to be manually added or removed from a Site. See Changing Site Membership Type for more information. |

**Site URL**

| Setting      | Description                                                                                                                    |
| ------------ | ------------------------------------------------------------------------------------------------------------------------------ |
| Friendly URL | Set a custom URL for Site pages. See Configuring Your Site’s Friendly URL for more information.                                |
| Pages        | Set Virtual Hosts that map to friendly URL for the Site’s pages. See Configuring Virtual Hosts Site URLs for more information. |

#### Third Party <a href="#third-party" id="third-party"></a>

Configure the PunchOut2Go Site setting for Commerce. For more information, see Commerce Connector to PunchOut2Go Reference Guide.

#### Third Party Applications <a href="#third-party-applications" id="third-party-applications"></a>

By default, Third Party Application includes the Maps and Google Places sections.

| Section            | Description                                                                                                                                                                                                                |
| ------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Asset Auto Tagging | When enabled, assets are auto tagged by the providers configured at the Instance scope. See Configuring Asset Auto Tagging for more information.                                                                           |
| Categorization     | Use this section to categorize your Site. This setting does not affect the Categories and Tags available for the Site. For more information on using Categories and Tags, see Organizing Content with Categories and Tags. |

#### Users <a href="#users" id="users"></a>

By default, the Users section includes the Default User Associations setting, where you can configure the default Roles and Teams for new Site members. For more information, see Configuring Role and Team Defaults for Site Members.

### Content and Data <a href="#content-and-data" id="content-and-data"></a>

By default, the Content and Data area includes the following settings:

* Assets
* Click to Chat
* Community Tools
* Digital Signature
* Documents and Media
* Message Boards
* Pages
* Recycle Bin
* Reports
* Sharing

#### Assets <a href="#assets" id="assets"></a>

| Section               | Description                                                                                                                                                                                                                              |
| --------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Maps                  | Select which maps API provider to use when displaying geo-localized assets in your Site. Geo-localized assets can be displayed for documents, web content articles, DDL records, and more. See Geo-locating Assets for more information. |
| Google Places API Key | Set the Google Places API Key.                                                                                                                                                                                                           |

#### Click to Chat <a href="#click-to-chat" id="click-to-chat"></a>

> Available: 7.4 +

You can integrate the portal with a number of live support chat platforms. The configuration at Site scope depends on the Click to Chat configuration at Instance scope. For more information, read Enabling Automated Live Chat Systems.

You can see the type of configuration for this setting at the Instance scope under the Site Settings Strategy text:

![Find the type of configuration at the Instance scope under Site Settings Strategy.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-settings/site-settings-ui-reference/images/23.png)

NOTE

Depending on your configuration at the Instance scope, you may not be able to update the configuration at Site scope. To verify your Instance Configuation, got to the Site Menu → Control Panel → Instance Settings → Content and Data → Click to Chat.

#### Community Tools <a href="#community-tools" id="community-tools"></a>

| Section  | Description                                                                                                                                  |
| -------- | -------------------------------------------------------------------------------------------------------------------------------------------- |
| Mentions | Determines whether to allow Users to mention other Users in Site applications. See Configuring Mentions for more information.                |
| Ratings  | Here you can select the type of ratings used for the different Site applications. See Configuring Content Rating Types for more information. |

#### Digital Signature <a href="#digital-signature" id="digital-signature"></a>

You can integrate the portal with DocuSign to manage electronic signatures. The configuration at Site scope depends on the Digital Signature configuration at Instance scope. You can check the Instance configuration under the Site Settings Strategy text:

![Find the type of configuration at the Instance scope under Site Settings Strategy.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-settings/site-settings-ui-reference/images/24.png)

NOTE

Depending on your Digital Signature configuration at the Instance scope, you may not be able to update the configuration at Site scope. To verify your Instance Configuation, go to Site Menu → Control Panel → Instance Settings → Content and Data → Digital Signature.

#### Documents and Media <a href="#documents-and-media" id="documents-and-media"></a>

When enabled, a User with view permission can browse the Site’s document library files and folders. For more information, see Securing Site Documents Content.

#### Message Boards <a href="#message-boards" id="message-boards"></a>

Using this option, you can enable automatic moderation of user messages on Message Boards after a predefined number of messages.

![Configure automatic moderation of Message Boards.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-settings/site-settings-ui-reference/images/25.png)

#### Pages <a href="#pages" id="pages"></a>

| Section                                  | Description                                                                                                                                                                                                                                                                                                                                |
| ---------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Google PageSpeed Insights (version 7.4+) | Enable the Google PageSpeed Insight integration in the portal. Using this option, you can audit your pages for accessibility issues and check suggestions for fixing them. See Analyzing SEO and Accessibility on Pages for more information.                                                                                              |
| Open Graph                               | These tags define page metadata to create engaging representations of your Site’s content when shared in applications that support the Open Graph protocol, such as Facebook, Slack, and Twitter. When enabled Open Graph `<meta>` tags are embedded in the `head` of the Site’s pages. For more information, read Configuring Open Graph. |
| Pages                                    | View the Site’s pages. On this section, you can also configure the propagation of changes from a Site template, if one exists. See Site Templates and Propagating Template Changes for more information.                                                                                                                                   |

#### Recycle Bin <a href="#recycle-bin" id="recycle-bin"></a>

Configure the Recycle Bin for a Site. You can specify the number of minutes an asset remains in the Recycle Bin before automatic deletion. By default, the max age for recycled items is 43200 minutes (30 days). See Configuring the Asset Recycle Bin for Sites for more information.

#### Reports <a href="#reports" id="reports"></a>

Settings for the Reports Group Service functionality.

#### Sharing <a href="#sharing" id="sharing"></a>

| Section         | Description                                                                                                                                                                                                                                                                                             |
| --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Content Sharing | Enable this option to allow child Sites to display content from this Site, including structures, templates, categories, or widget templates. When you disable this option, you immediately revoke content sharing from all child Sites. See Managing Content Sharing Across Sites for more information. |
| Sharing         | When enabled, Users can share items with one another. See Enabling and Configuring Sharing for more information.                                                                                                                                                                                        |

### [Other](https://learn.liferay.com/web/guest/w/dxp/site-building/site-settings/site-settings-ui-reference#other) <a href="#other" id="other"></a>

By default, the Other area includes the following settings:

| Section           | Description                                                                                                                            |
| ----------------- | -------------------------------------------------------------------------------------------------------------------------------------- |
| Other             | Includes custom Site settings you have defined for your Site, if any. For more information, read Setting and Accessing Configurations. |
| Tax               | Configure the settings for the Remote Commerce Tax Configuration in Commerce. For more information, see the Commerce documentation.    |
| category.tax-rate | Configure the settings for the Commerce Avalara integration. For more information, see the Commerce documentation.                     |

### Site Setting UI Reference in versions 7.2 and 7.3 <a href="#site-setting-ui-reference-in-liferay-dxp-72-and-73" id="site-setting-ui-reference-in-liferay-dxp-72-and-73"></a>

To view a Site’s settings, open the Site Menu (![Site Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu.png)) and go to _Configuration_ → _Settings_, where you’ll see the following tabs:

* General
* Social
* Language
* Advanced

![Site Settings are organized into four tabs.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-settings/site-settings-ui-reference/images/01.png)

NOTE

Many of these settings can be localized to provide translations based on a user’s locale. See Introduction to Localization and Modifying Localizable Site Fields for more information.

#### General <a href="#general" id="general"></a>

General settings range from core configuration, like a Site’s Membership Type, to finer details like Documents and Media indexing options. The tab’s content is organized into the following subsections: Details, Pages, Categorization, Site URL, Documents and Media, Open Graph, Asset Auto Tagging, Sharing, and Custom Fields.

**Details**

**Site ID**: A unique number automatically generated for a Site at its creation. This ID is permanent and cannot be changed.

**Name**: Set a Site’s title using the _Name_ field. This title is displayed in the browser’s title bar as well as the header for each Site page. You can also localize a Site’s name via the _Language Flag_ button.

**Description**: Use the _Description_ field to explain a Site’s purpose. You can also localize a Site’s description via the _Language Flag_ button.

**Active**: Determine whether a Site is _Active_ or _Inactive_. While inactive, a site is inaccessible to users, though it can be reactivated if desired.

**Membership Type**: A _Membership Type_ determines how restrictive a Site’s membership is. See Changing Site Membership Types for more information.

**Allow Manual Membership Management**: Determine whether to allow members to be manually added or removed from a Site. If your Site’s membership is handled automatically by a membership policy, you can disable this setting. See Managing Membership Policies for Sites for more information.

**Parent Site**: Designate a Site as a child Site by selecting its parent Site. See Site Hierarchies for more information.

**Limit Membership to Parent Site Members**: Determine whether a child Site’s membership is limited to members of its parent Site. This option only appears for child Sites.

**Pages**

View a Site’s Public and Private Pages, if any exist, and enable or disable the propagation of changes from the selected Site template. If they don’t exist, a Site Templates selector appears for creating pages with a template.

![In the Pages section, you can view a Site's Public and Private Pages.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-settings/site-settings-ui-reference/images/03.png)

#### Custom Fields <a href="#custom-fields" id="custom-fields"></a>

View and configure any custom fields you’ve defined for your pages. With these fields, you can set page metadata, such as author, date of creation, and geolocation. This section only appears once you’ve configured custom fields for your Site. See Custom Fields for more information.

**Categorization**

Use Categories and Tags to categorize a Site’s content so users can more easily find it. For more information on using tags and categories, see Organizing Content with Tags and Categories.

**Site URL**

**Friendly URL**: Set a custom URL for both public and private Site pages. See Configuring Your Site’s Friendly URL for more information.

**Public and Private Virtual Host**: Set public and private Virtual Hosts to map to a Site’s Public and Private Pages. See Configuring Virtual Hosts Site URLs for more information.

![In the Site URL section, you can configure your Site's Friendly URL, as well as Virtual Hosts for your Public and Private Pages.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-settings/site-settings-ui-reference/images/05.png)

**Documents and Media**

Determine whether to enable Directory Indexing for a Site. When enabled, a User with view permission can browse the Site’s document library files and folders.

**Site Template**

If you created your Site using a _Site Template_, it’s displayed here, along with whether the template allows Users to modify pages that inherit it. See Site Templates for more information.

![View your page's selected template.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-settings/site-settings-ui-reference/images/07.png)

**Open Graph**

**Enable Open Graph**: Determine whether to embed Open Graph `<meta>` tags in the `<head>` of your Site’s pages. These tags define page metadata to create engaging representations of your Site’s content when shared in applications that support the Open Graph protocol, such as Facebook, Slack, and Twitter.

**Image**: Use the Image field to define the following Open Graph `<meta>` properties for a page:

```html
   <meta property="og:image" content="http://example.com/ogp.jpg" />
   <meta property="og:image:secure_url" content="https://secure.example.com/ogp.jpg" />
   <meta property="og:image:type" content="image/jpeg" />
   <meta property="og:image:width" content="400" />
   <meta property="og:image:height" content="300" />
```

**Image Alt Description**: Use the Image Alt Description field to define the `og:image:alt` property for a page. You can also localize an image’s alt description via the _Language Flag_ button.

See Configuring Open Graph to learn more.

![You can enable or disable Open Graph, define image meta tags, and see an image preview.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-settings/site-settings-ui-reference/images/08.png)

**Sharing**

Determine whether to enable document sharing between Site Users. When enabled, Users can share items with one another. See Sharing Documents with Other Users for more information.

**Asset Auto Tagging**

Determine whether to enable asset auto tagging for a Site. When enabled, assets are auto tagged by the providers configured at Instance scope. See Configuring Asset Auto Tagging for more information.

#### Social <a href="#social" id="social"></a>

Here you can manage the social interactions between Users on a Site.

**Ratings**

Here you can select the type of ratings used for the following Site applications: Comments, Knowledge Base, Blogs, Wiki, Message Boards, Web Content, and Documents and Media. See Configuring Content Ratings Types for more information.

![You can select the type of ratings used for Site applications.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-settings/site-settings-ui-reference/images/11.png)

**Mentions**

Determine whether to allow Users to mention other Users in Site applications. See Mentioning Users for more information.

#### Language <a href="#language" id="language"></a>

Here you can choose whether to use the installation’s default language options, or define your own for a Site.

![You can update language options through the Languages tab of Site Settings.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-settings/site-settings-ui-reference/images/13.png)

#### Advanced <a href="#advanced" id="advanced"></a>

Here you can configure additional settings, including analytics services, content sharing, and more.

**Default User Associations**

Select the Roles and Teams that new Site members are assigned to by default. See Configuring Role and Team Defaults for Site Members for more information.

![You can select the Roles and Teams that new Site members are assigned to by default.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-settings/site-settings-ui-reference/images/14.png)

**Analytics**

Use the provided fields to set a Site’s Google Analytics ID and configure additional Google Analytics options. You can also set the Piwik Analytics tracking code for a Site by entering the full script code, including script start and end tags.

If you require a different analytics service, you can add it. See Adding a New Analytics Service for more information and how to add additional fields for different analytics services.

![In the Analytics section, you can configure a Site's analytic services.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-settings/site-settings-ui-reference/images/15.png)

**Maps**

Select which maps API provider to use when displaying geolocalized assets in your Site. Geolocalized assets can be displayed for documents, web content articles, DDL records, and more. See Geolocating Assets for more information.

![You can select the maps API provider used for Site geolocation.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-settings/site-settings-ui-reference/images/16.png)

**Recycle Bin**

Determine whether to enable the Recycle Bin for a Site. You can specify the number of minutes an asset remains in the Recycle Bin before being automatically deleted. By default, the max age for recycled items is 43200 minutes (i.e., 30 days). See Configuring the Asset Recycle Bin for Sites for more information.

![You can configure a Site's Recycle Bin.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-settings/site-settings-ui-reference/images/17.png)

**Content Sharing**

Determine whether child Sites can display content from this Site, including structures, templates, categories, widget templates, and more. Disabling this option immediately revokes content sharing from all child Sites. See Managing Content Sharing Across Sites for more information.

![You can determine whether child Sites can display content from this Site.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-settings/site-settings-ui-reference/images/18.png)

\
