# Working with Search Pages

## [Search Pages](https://learn.liferay.com/web/guest/w/dxp/using-search/search-pages-and-widgets/working-with-search-pages/search-pages#search-pages) <a href="#search-pages" id="search-pages"></a>

Access the default search page at [`localhost:8080/web/guest/search`](http://localhost:8080/web/guest/search). If you have a running Liferay DXP with some content in it, you can navigate there and execute a search. By default, this page is a hidden page in the default site. Users are routed to this hidden page when they enter search terms in a _Search Bar_ widget. The search page is also where users browse search results.

![The default search page is hidden and accessible by direct URL or by using the Search Bar widget.](https://resources.learn.liferay.com/images/dxp/latest/en/using-search/search-pages-and-widgets/working-with-search-pages/search-pages/images/05.png)

Users can opt to customize search experience by creating a new Search page, adding different Search Widgets, and tweaking a variety of available widget configurations to provide a precise and tailored search experience for a site’s users. Using the [default](https://learn.liferay.com/web/guest/w/dxp/using-search/search-pages-and-widgets/working-with-search-pages/search-pages#default-search-pages) search page can be a good starting point to creating your own custom search pages.

NOTE

Newly created sites do not get a Search Page created by default. You’ll need to create one. You can use an existing Search Page Template or create a new Search Page yourself.

![A new, blank site needs a search page to be created and configured.](https://resources.learn.liferay.com/images/dxp/latest/en/using-search/search-pages-and-widgets/working-with-search-pages/search-pages/images/01.png)

### [Default Search Pages](https://learn.liferay.com/web/guest/w/dxp/using-search/search-pages-and-widgets/working-with-search-pages/search-pages#default-search-pages) <a href="#default-search-pages" id="default-search-pages"></a>

Out of the box (default site, theme, and search configurations) the search experience has two components for end users:

1. A search bar embedded in the header of each page.
2. A default search page where search requests are routed and results are displayed.

Behind the scenes, The search bar widget points to a hidden search page with the friendly URL `/search`.

![A destination page must be configured for a search bar.](https://resources.learn.liferay.com/images/dxp/latest/en/using-search/search-pages-and-widgets/working-with-search-pages/search-pages/images/02.png)

Entering a search query redirects to the default search page, where results are displayed in the Search Results widget.

![The default search page provides a full search experience.](https://resources.learn.liferay.com/images/dxp/latest/en/using-search/search-pages-and-widgets/working-with-search-pages/search-pages/images/03.png)

The default search page contains the Search Bar, Search Results, Search Suggestions, Search Options, and several Search Facet widgets.. In the example above, the Type facet, User fact, and Last Modified facet can be seen in the left sidebar. Read more about the different [search facets](https://learn.liferay.com/web/guest/w/dxp/using-search/search-pages-and-widgets/search-facets) that are available and how they can be customized.

The default search page is based on the Search page template, but it doesn’t inherit changes from the template by default. That means you can customize the search page directly without changing the template’s inheritance configuration.

![By default, the Search page doesn't inherit changes from the page template.](https://resources.learn.liferay.com/images/dxp/latest/en/using-search/search-pages-and-widgets/working-with-search-pages/search-pages/images/04.png)

If you require just a few changes to the default page, don’t abandon it and create one manually. Just make the configuration changes you need, including adding, configuring, and removing widgets on the page. On the other hand, if you want a clean break from the default search page, starting from scratch is an option.

### [Creating a Search Page](https://learn.liferay.com/web/guest/w/dxp/using-search/search-pages-and-widgets/working-with-search-pages/search-pages#creating-a-search-page) <a href="#creating-a-search-page" id="creating-a-search-page"></a>

You can create search pages with a page template, or use a manually configured search page for a single Site. See [Using a Search Page Template](https://learn.liferay.com/web/guest/w/dxp/using-search/search-pages-and-widgets/working-with-search-pages/using-a-search-page-template) to learn about search page templates; see [Creating a Search Page](https://learn.liferay.com/web/guest/w/dxp/using-search/search-pages-and-widgets/working-with-search-pages/creating-a-search-page) to learn about configuring a search page individually per Site.

After choosing your approach and reading here to get your search page up and running, read through the more detailed articles on the various search widgets and the configuration documentation to understand the full suite of configuration options.
