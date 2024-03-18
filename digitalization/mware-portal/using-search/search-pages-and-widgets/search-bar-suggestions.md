# Search Bar Suggestions

Liferay’s Search Bar Suggestions provides _search as you type_ functionality. As you type search keywords into the Search Bar widget, the query is processed by the search engine, and results are returned in a drop-down list. You can click one of the results from the list or execute the search as usual by clicking the magnifying glass icon or the Enter key to view the results in the Search Results widget.

![As you type, search results appear in a drop-down list.](https://resources.learn.liferay.com/images/dxp/latest/en/using-search/search-pages-and-widgets/search-bar-suggestions/images/01.png)

IMPORTANT

The search bar must be configured to use the default display template (i.e., [widget template](https://learn.liferay.com/web/guest/w/dxp/site-building/displaying-content/additional-content-display-options/styling-widgets-with-widget-templates)) to work with search bar suggestions.

## [Configuring Search Bar Suggestions](https://learn.liferay.com/web/guest/w/dxp/using-search/search-pages-and-widgets/search-bar-suggestions/configuring-search-bar-suggestions#configuring-search-bar-suggestions) <a href="#configuring-search-bar-suggestions" id="configuring-search-bar-suggestions"></a>

Search Bar Suggestions adds _search as you type_ functionality to Liferay’s Search Bar widget.

![Select a result from the drop-down list or execute a search as usual.](https://resources.learn.liferay.com/images/dxp/latest/en/using-search/search-pages-and-widgets/search-bar-suggestions/configuring-search-bar-suggestions/images/01.gif)

The user types search keywords into the Search Bar as usual. The keywords are ingested by Liferay and sent to the search engine; results are returned in a drop-down list. Continuing to type causes additional queries to be sent. You can click one of the results from the list or execute the search as usual by clicking the magnifying glass icon or the Enter key to view the results in the Search Results widget.

### [Configuration Options](https://learn.liferay.com/web/guest/w/dxp/using-search/search-pages-and-widgets/search-bar-suggestions/configuring-search-bar-suggestions#configuration-options) <a href="#configuration-options" id="configuration-options"></a>

If Search Bar Suggestions is enabled in [Instance Settings](https://learn.liferay.com/web/guest/w/dxp/using-search/search-pages-and-widgets/search-bar-suggestions/configuring-search-bar-suggestions#search-bar-suggestions-configuration-scope), you can configure Search Suggestions at the widget [scope](https://learn.liferay.com/web/guest/w/dxp/system-administration/configuring-liferay/understanding-configuration-scope).

The primary way to configure Search Bar Suggestions is with the Search Bar widget’s configuration.

![Use the Suggestions Configuration of the Search Bar widget's configuration screen.](https://resources.learn.liferay.com/images/dxp/latest/en/using-search/search-pages-and-widgets/search-bar-suggestions/configuring-search-bar-suggestions/images/02.png)

**Enable Suggestions:** Suggestions are enabled by default. Uncheck the checkbox to turn off suggestions.

**Character Threshold for Displaying Suggestions:** Each time the configured number of characters is entered into the Search Bar, the query is re-processed and the results are refreshed accordingly. Tune this setting to balance sending lots of search requests (i.e., setting the threshold too low triggers many search requests) with usability (i.e., setting the threshold too high means the user must type a lot to trigger the first suggestion query).

**Suggestions Contributor Configuration:** Configure the contributor(s) that control the query sent to the search engine. This setting is made up of three values:

1. **Suggestions Contributor**: The default contributor, and the only one available out-of-the-box in Liferay CE, is `Basic`. In Liferay DXP you can choose to use one or more Blueprint contributors instead. See [Search Bar Suggestions Blueprints](https://learn.liferay.com/web/guest/w/dxp/using-search/search-pages-and-widgets/search-bar-suggestions/search-bar-suggestions-blueprints) for more information.
2. **Display Group Name**: The default is `suggestions`. This value is a language key that resolves to Suggestions as the heading in the UI for the English language. You can change the language key or add your own. For example, make it `is-this-what-you're-looking-for`, then add the [translation](https://learn.liferay.com/web/guest/w/dxp/system-administration/configuring-liferay/changing-language-translations#adding-a-language-key-and-translation). If you are using more than one Blueprint contributor, make sure each one has a unique Display Group Name value.
3. **Size**: Set the maximum number of results to display in the Search Bar drop-down list. The default is `5`. Because display space is limited, there’s no benefit to setting the size to a large number.

TIP

Search Blueprints users can choose to use a Blueprint to control the Search Bar Suggestions query. When _Blueprint_ is selected as the contributor, additional configuration options appear in the Search Bar widget configuration. See [Search Bar Suggestions Blueprints](https://learn.liferay.com/web/guest/w/dxp/using-search/search-pages-and-widgets/search-bar-suggestions/search-bar-suggestions-blueprints) for more information.

### [Configuring Search Bar Suggestions in the Control Panel](https://learn.liferay.com/web/guest/w/dxp/using-search/search-pages-and-widgets/search-bar-suggestions/configuring-search-bar-suggestions#configuring-search-bar-suggestions-in-the-control-panel) <a href="#configuring-search-bar-suggestions-in-the-control-panel" id="configuring-search-bar-suggestions-in-the-control-panel"></a>

There are three Control Panel configuration entries that can affect Search Bar Suggestions:

1. Service Access Policies
2. Search Suggestions in System Settings
3. Search Bar in System and Instance Settings

#### [Configuring the Suggestions Service Access Policy](https://learn.liferay.com/web/guest/w/dxp/using-search/search-pages-and-widgets/search-bar-suggestions/configuring-search-bar-suggestions#configuring-the-suggestions-service-access-policy) <a href="#configuring-the-suggestions-service-access-policy" id="configuring-the-suggestions-service-access-policy"></a>

[Service Access Policies](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/securing-liferay/securing-web-services/setting-service-access-policies#understanding-service-access-policies) control access to Liferay’s services. The policy for search bar suggestions is enabled by default, allowing unauthenticated users to access the REST service endpoint that returns the suggestions. In short, it enables search bar suggestions for guest users as they type in the search bar.

To configure the Service Access Policy,

1. Open the Global Menu → Control Panel → Service Access Policy (in the Security section).
2. Click the _SEARCH\_SUGGESTIONS_ entry.

If you disable the policy, guests do not see suggestions, even if suggestions is enabled in the widget configuration.

#### [Configuring the Suggestions REST Endpoint](https://learn.liferay.com/web/guest/w/dxp/using-search/search-pages-and-widgets/search-bar-suggestions/configuring-search-bar-suggestions#configuring-the-suggestions-rest-endpoint) <a href="#configuring-the-suggestions-rest-endpoint" id="configuring-the-suggestions-rest-endpoint"></a>

The REST endpoint that provides the Search Bar Suggestions functionality can be disabled if necessary, using System or Instance Settings.

To configure the Search Suggestions endpoint,

1. Go to System or Instance Settings in the Global Menu → Control Panel → Configuration section.
2. In the Platform → Search category, open the Search Suggestions entry.
3. There’s just one setting: Enabled.

When disabled, calls to the required endpoint are ignored; thus, none of the other configurations for Search Bar Suggestions are taken into consideration. In fact, the Search Bar widget configuration doesn’t display the Search Bar Suggestions settings. The feature is entirely disabled throughout the system or instance.

#### [Configuring Suggestions with the Search Bar](https://learn.liferay.com/web/guest/w/dxp/using-search/search-pages-and-widgets/search-bar-suggestions/configuring-search-bar-suggestions#configuring-suggestions-with-the-search-bar) <a href="#configuring-suggestions-with-the-search-bar" id="configuring-suggestions-with-the-search-bar"></a>

Most configuration of Search Bar Suggestions occur at the widget scope. In addition to the Search Bar widget’s configuration, you can configure the Search Bar Suggestions defaults at the system and instance [scope](https://learn.liferay.com/web/guest/w/dxp/system-administration/configuring-liferay/understanding-configuration-scope).

![Configure Search Bar Suggestions at the instance or system scope.](https://resources.learn.liferay.com/images/dxp/latest/en/using-search/search-pages-and-widgets/search-bar-suggestions/configuring-search-bar-suggestions/images/03.png)

Here’s what happens when you enable or disable Search Bar Suggestions at the system, instance, or widget [scope](https://learn.liferay.com/web/guest/w/dxp/system-administration/configuring-liferay/understanding-configuration-scope):

| Configuration Scope | Enabled? | Search Bar widget result                                                                                                           |
| ------------------- | -------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| System Settings     | ✘        | Sets the default for all instances as disabled. Suggestions settings do not appear in the widget configuration.                    |
| Instance Settings   | ✘        | Suggestions settings do not appear in the widget configuration.                                                                    |
| Instance Settings   | ✔        | Suggestions are enabled, and settings appear in the widget configuration. Suggestions can be disabled in the widget configuration. |

The other settings work according to Liferay’s [configuration scope paradigm](https://learn.liferay.com/web/guest/w/dxp/system-administration/configuring-liferay/understanding-configuration-scope): settings at higher scopes like system and instance set the defaults, whereas the more granular widget scope can override those defaults.



## [Search Bar Suggestions Blueprints](https://learn.liferay.com/web/guest/w/dxp/using-search/search-pages-and-widgets/search-bar-suggestions/search-bar-suggestions-blueprints#search-bar-suggestions-blueprints)

Search Bar Suggestions uses the regular [search bar query](https://learn.liferay.com/web/guest/w/dxp/using-search/search-pages-and-widgets/search-insights) by default. [Liferay Enterprise Search users](https://learn.liferay.com/web/guest/w/dxp/using-search/liferay-enterprise-search/activating-liferay-enterprise-search) can influence or override the query using [Search Blueprints](https://learn.liferay.com/web/guest/w/dxp/using-search/liferay-enterprise-search/search-experiences/search-blueprints/understanding-search-blueprints).

First, [build a Blueprint](https://learn.liferay.com/web/guest/w/dxp/using-search/liferay-enterprise-search/search-experiences/search-blueprints/creating-and-managing-search-blueprints). Once you have a Blueprint,

1. Make sure Search Bar Suggestions are enabled in Instance Settings. They are enabled by default.
2. Configure Search Bar Suggestions in the Search Bar widget configuration.
   * Click the Options (![Options](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-widget-options.png)) menu and choose _Configuration_.
3. Change the Suggestions Contributor to _Blueprint_.
4. Select the Blueprint by clicking _Select_.
5. Click _Save_.

Now the Search Bar Suggestions use the Blueprint-provided query clauses and configurations to send a customized request to Elasticsearch.

There are a few additional configurations you can use.

### [Configuring Search Bar Suggestions to use a Blueprint](https://learn.liferay.com/web/guest/w/dxp/using-search/search-pages-and-widgets/search-bar-suggestions/search-bar-suggestions-blueprints#configuring-search-bar-suggestions-to-use-a-blueprint) <a href="#configuring-search-bar-suggestions-to-use-a-blueprint" id="configuring-search-bar-suggestions-to-use-a-blueprint"></a>

When you select the Blueprint Suggestions Contributor, new configuration fields appear in the Search Bar Widget Configuration.

![The Blueprint contributor uses additional configurations.](https://resources.learn.liferay.com/images/dxp/latest/en/using-search/search-pages-and-widgets/search-bar-suggestions/search-bar-suggestions-blueprints/images/01.png)

**Blueprint:** This is the only required additional setting. Select the Blueprint to use.

**Include Asset URL:** Enable this to return the link to the result with the response. If an asset appears on a page accessible to the search user, clicking a result brings the user to that display page. Otherwise the asset is shown in an Asset Publisher under the search page’s context. If disabled, clicking on results in the Suggestions list does nothing.

**Include Asset Summary:** Enable this to return the [summary](https://learn.liferay.com/web/guest/w/dxp/using-search/search-pages-and-widgets/search-results/search-results-behavior#result-summaries) of the result with the response. Disable this to return a more concise list of results, excluding the summary built by the search framework.

**Fields:** Define additional indexed fields to also return with the response. This is used when creating a custom [widget template](https://learn.liferay.com/web/guest/w/dxp/site-building/displaying-content/additional-content-display-options/styling-widgets-with-widget-templates#creating-a-widget-template) that requires the use of certain index fields and their values.

\
\
