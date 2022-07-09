# Advanced Site Settings

Advanced Settings relate to security (like User Roles) or require external configuration (like creating a Google Analytics account) to use.

#### DEFAULT USER ASSOCIATIONS <a href="#default-user-associations" id="default-user-associations"></a>

_Default User Associations_ configures Site roles and teams that newly assigned Site members have by default.&#x20;

### Analytics <a href="#analytics" id="analytics"></a>

MWARE Portal includes built-in support for Google Analytics for analyzing traffic on your Site. Google Analytics provides a snippet of code which you add to your pages enable tracking. Adding this code to every page on a Site would be tedious, especially if it’s a large Site with a lot of user-generated content.

There are two ways to mitigate this problem:

1. A web developer can hard-code the tracking code into a theme, which embeds it on every page.
2. An administrator can enter the tracking code in Site settings.

To use option #2:

1. Go to _Configuration_ → _Settings_ → _Advanced_.
2. Expand the _Analytics_ section.
3. Enter your Google Analytics ID.
4.  Click _Save_.

    All the pages in the Site you selected now have the Google Analytics code and can be tracked.

![Figure 1: To set up Google Analytics: sign up, receive an ID, and then enter it into the Google Analytics ID field.](https://liferay-support.zendesk.com/hc/article\_attachments/360030158251/maintaining-google-analytics.png)

To enable a different analytics service:

1. Go to _Configuration_ in the Control Panel.
2. Go to _Instance Settings_ → _Analytics_.
3. Enter the name of any additional service you want to add in the _Analytics_ field provided.
4. Once you have entered the name, go to the _Site Settings_ → _Advanced_ → _Analytics_ page for the Site where you wish to add analytics.
5. Copy the JavaScript tracking code provided by your analytics platform into the corresponding field for your service.

Now all pages on the selected Site contain the tracking script and send analytics data to your analytics platform.

### Maps <a href="#maps" id="maps"></a>

The _Maps_ option configures the maps API provider used by your portal instance when displaying geolocalized assets. Geolocalized assets can be displayed for documents, web content articles, DDL records, etc. Maps is available under the Advanced tab.&#x20;

#### RECYCLE BIN <a href="#recycle-bin" id="recycle-bin"></a>

The _Recycle Bin_ option enables or disables the Recycle Bin for your Site. You can also regulate the age (in minutes) for which content is able to be stored in the Recycle Bin until it is permanently deleted.&#x20;

#### CONTENT SHARING <a href="#content-sharing" id="content-sharing"></a>

If you select the _Content Sharing_ tab from the Advanced tab, you can configure whether sub-Sites can display content from this Site. Administrators of this Site’s sub-Sites can use all structures, templates, categories, widget templates, and more from this parent Site. Even if you initially allowed content sharing between the parent Site and its sub-Sites, you can disable this option and immediately revoke content sharing from all sub-Sites.

You can manage this globally by navigating to the Control Panel → _Configuration_ → _Instance Settings_ → _Content & Data_ → _Sharing_ → _Content Sharing_. First, you can choose if Site administrators can display content in Sites from other Sites they administer. For example, suppose that a certain User is a Site administrator of two Sites: _Engineering_ and _Marketing_. The checkbox in the Content Sharing section of Instance Settings determines if the Site administrator can display content from the Marketing Site in the Engineering Site and vice versa.

You can also choose if child Sites can display content from parent Sites and configure the defaults. There are three options:

**Enabled by Default**: Child Sites can display content from parent Sites by default, but this can be disabled by a Site administrator.

**Disabled by Default**: Child Sites cannot display content from parent Sites by default, but this can be enabled by a Site administrator.

**Disabled**: Child Sites cannot display content from parent Sites, and this behavior cannot be changed by a Site administrator.

That covers your Site’s advanced settings. You’re now equipped to manage all aspects of your Site’s configuration.
