# Configuring Search Pages

There are multiple ways to skin the search cat (disclaimer: no actual cats were harmed during the writing of this article).

If you’re unsure which approach to take, use the default configuration. It provides a sensible starting point that can be modified later, as needed.

If you’re in need of a fully customized experience, manually configure the search experience.

After choosing your approach and reading here to get it up and running, find the articles on the Search Bar, Search Facets, and Search Results to understand the full suite of configuration options.

Search display pages are where users go to enter search terms and browse search results.

### Search Page Templates <a href="#search-page-templates" id="search-page-templates"></a>

The default search page is backed by a Search Page Template, and manually configured search pages can use the template, too. The template can be used in two ways:

1. Enable inheriting changes to the template, if you want the search page to get any updates made to the template at a later date.
2. Create the page based on the template, but independently configured after the initial creation.

Out of the box, the Search Page Template includes these widgets:

* Search Bar
* Suggestions
* Search Results
* Search Options
* Site Facet: This widget is hidden unless the Search Bar is configured to search the _Everything_ scope and results from multiple sites are returned.
* Type Facet
* Tag Facet
* Category Facet
* Folder Facet
* User Facet
* Modified Facet

Out of the box, widgets use the _Barebone_ Application Decorators: unless there’s content to render in the widget, the widget body is hidden. The header is displayed if you hover over it.

![Figure 1: At first glance, not much is happening on the search page. But, theres more than meets the eye.](https://liferay-support.zendesk.com/hc/article\_attachments/360036301411/search-barebone-widgets.png)

Because of this, when you visit a search page created from the default search page template, you won’t see certain widgets fully rendered.

By contrast, when you add a search widget to a page manually, they use the _Borderless_ decorator (by default), which shows more of the widget even when there is no content to display.

### Default Search Pages <a href="#default-search-pages" id="default-search-pages"></a>

Using the default site and the default theme with the default search settings, the out-of-the-box search experience has two components for end users:

1. A search bar embedded on each page.
2. A default search page where search requests are routed.

Behind the scenes, The search bar widget points to a hidden search page with the friendly URL `/search`.

![Figure 2: By default, the embedded search bar points to the pre-configured /search destination page.](https://liferay-support.zendesk.com/hc/article\_attachments/360036301371/search-dest-page.png)

Enter a search term and you’re redirected to the default search page, where results are displayed in the Search Results widget.

![Figure 3: The default page is pre-configured with the Search Results widget and the various Facet widgets to provide a full search experience.](https://liferay-support.zendesk.com/hc/article\_attachments/360036301431/search-default-page.png)

The default search page is based on a Search page template, but it doesn’t inherit changes from the page template by default. That means you can customize the search page directly without changing the template’s inheritance configuration.

![Figure 4: Configure the Search page. By default, it doesnt inherit changes from the page template.](https://liferay-support.zendesk.com/hc/article\_attachments/360036302392/search-page-config.png)

If you require just a few changes to the default page, don’t abandon it and create one manually. Just make the configuration changes you need, including adding, configuring, and removing widgets on the page. On the other hand, if you want a clean break from the default search page, starting from scratch is also an option.

### Manual Search Page Configuration <a href="#manual-search-page-configuration" id="manual-search-page-configuration"></a>

It’s reasonable to create the search experience from the ground up. If you’re working from a newly created site, it’s a necessity. These steps show you how to switch to a manually configured search experience in the default site, but you can skip the step on deleting the default search page if you’re starting with a new site:

1.  Delete the existent search page by navigating to the default site’s menu and clicking _Site Builder_ → _Pages_. Click the Search page’s Actions menu icon (![Actions](https://liferay-support.zendesk.com/hc/article\_attachments/360036302332/icon-actions.png)) and select _Delete_. Confirm you want to delete the page, and it’s gone.

    Once deleted, the search bar disappears from your site pages, replaced by a warning message visible only to site administrators:

    <img src="https://liferay-support.zendesk.com/hc/article_attachments/360036301391/search-bar-warning.png" alt="Figure 5: The search bar is only visible if it points to an existent page." data-size="original">

    Figure 5: The search bar is only visible if it points to an existent page.
2.  Create a new page named whatever you want (_Finders Keepers_, perhaps). Make it hidden or add it to the navigation as you please (the default search page is hidden from the navigation).

    If you want a pre-configured search page, create it from the Search page template. Find the template in the Add Site Page form. It’s under _Global Templates_.

    <img src="https://liferay-support.zendesk.com/hc/article_attachments/360036302352/search-page-template.png" alt="Figure 6: Theres a handy page template for creating search pages." data-size="original">

    Figure 6: There's a handy page template for creating search pages.
3. If you’re creating a page not backed by the template, add and configure all the widgets you need. You’ll find all the available search widgets in the Add Widget menu’s Search section. Lay them out however you want on the page.
4.  Configure the search bar at the top of the page, making sure it points to your new search page’s friendly URL (for example, `/finders-keepers`).

    Click the Search Bar widget’s Options menu (![Options](https://liferay-support.zendesk.com/hc/article\_attachments/360036302372/icon-app-options.png)).

    Click _Configuration_ and set the Destination Page to the search page’s friendly URL.

    Click _Save_.

Now your search page is up and running.
