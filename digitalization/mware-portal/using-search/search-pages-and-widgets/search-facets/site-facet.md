# Site Facet

The Site Facet narrows search results down to those existing in a certain Site. Each Site with content matching the searched keyword appears as a facet term.

![Example of Site facet results.](https://resources.learn.liferay.com/images/dxp/latest/en/using-search/search-pages-and-widgets/search-facets/site-facet/images/01.png)

IMPORTANT

For the Site Facet to display multiple sites, the Search Bar must be configured to search _Everything_. If not searching for Everything, only the current Site is searched, and the Site Facet has nothing to display. When this occurs, the Site Facet is hidden on the page.

Configuring the globally embedded page-top Search Bar to search for Everything not only configures the embedded Search Bar on all pages. It also ensures that the Search Page’s Search Bar searches Everything, because the page-top Search Bar’s configuration overrides the Search Page’s Search Bar configuration. The same does not apply to other Search Bar widgets in the Site. Each of these must be configured as desired.

If the global Search Bar is disabled, configure the Search Page’s Search Bar widget to search for Everything.

To configure the search scope,

1.  Click on Options icon (![Options icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-app-options.png)) of the search bar and click _Configuration_.

    ![Click on the Configuration option.](https://resources.learn.liferay.com/images/dxp/latest/en/using-search/search-pages-and-widgets/search-facets/site-facet/images/02.png)
2.  Set the Scope option to _Everything_.

    ![Select everything in the drop down menu.](https://resources.learn.liferay.com/images/dxp/latest/en/using-search/search-pages-and-widgets/search-facets/site-facet/images/03.png)

    Once finished, click the _Save_ button.

### Configuring the Site Facet <a href="#configuring-the-site-facet" id="configuring-the-site-facet"></a>

To configure the Site Facet, open the Options menu (![Click on the options icon of the search bar.](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-app-options.png)) of the facet and click _Configuration_.

![Click on the Configuration option.](https://resources.learn.liferay.com/images/dxp/latest/en/using-search/search-pages-and-widgets/search-facets/site-facet/images/04.png)

**Display Settings:** Choose between _Default_, _Compact Layout_, and _Label Layout_. The Default layout shows checkboxes next to each term but the Compact layout does not. The Label layout shows small clickable labels for each term.

The Advanced Configuration section contains additional options:

**Site Parameter Name:** Change the parameter name in the URL for the Facet. The default is _site_.

**Max Terms:** Set the maximum number of facet terms to display, regardless of how many matching terms are found for the facet.

**Frequency Threshold:** Set the minimum frequency required for terms to appear in the list of facet terms. For example, if the frequency threshold of a facet is set to 3, a term with two matching results doesn’t appear in the term result list.

**Display Frequencies:** Choose whether or not to display the term frequencies.

**Order Terms By:** For 7.4 +, you can choose the facet term sorting strategy: Term Frequency Descending (default), Term Frequency Ascending, Term Value Ascending, or Term Value Descending. The default sorting orders facet terms from most matches to least. Term value options order facet terms alphabetically.

Once finished, click the _Save_ button.

\
