# Type Facet

The Type Facet narrows search results down to those associated with a certain Asset Type. Each Type with content matching the searched keyword appears as a facet term.

![Example of type facet results.](https://resources.learn.liferay.com/images/dxp/latest/en/using-search/search-pages-and-widgets/search-facets/type-facet/images/01.png)

By default, all available Asset Types are displayed as facet terms:

* Blogs Entry
* Calendar Event
* Document
* Documents Folder
* Dynamic Data Lists Record
* Form Record
* Knowledge Base Article
* Message Boards Message
* Page
* User
* Web Content Article
* Web Content Folder
* Wiki Page

### [Configuring the Type Facet](https://learn.liferay.com/web/guest/w/dxp/using-search/search-pages-and-widgets/search-facets/type-facet#configuring-the-type-facet) <a href="#configuring-the-type-facet" id="configuring-the-type-facet"></a>

To configure the Type Facet, open the Options menu (![options icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-app-options.png)) of the facet and click _Configuration_.

![Click on the Configuration option.](https://resources.learn.liferay.com/images/dxp/latest/en/using-search/search-pages-and-widgets/search-facets/type-facet/images/02.png)

**Display Settings:** Choose between _Default_, _Compact Layout_, and _Label Layout_. The Default layout shows checkboxes next to each term but the Compact layout does not. The Label layout shows small clickable labels for each term.

The Advanced Configuration section contains additional options:

**Type Parameter Name:** Change the parameter name in the URL for the Facet. The default is _type_.

**Frequency Threshold:** Set the minimum frequency required for terms to appear in the list of facet terms. For example, if the frequency threshold of a facet is set to 3, a term with two matching results doesn’t appear in the term result list.

**Display Frequencies:** Choose whether or not to display the term frequencies.

**Order Terms By:** For Liferay 7.4 U60+/GA60+, you can choose the facet term sorting strategy: Term Frequency Descending (default), Term Frequency Ascending, Term Value Ascending, or Term Value Descending. The default sorting orders facet terms from most matches to least. Term value options order facet terms alphabetically.

**Current and Available:** Choose which asset types are visible in the facet. To remove types, select from the Current section by clicking and highlighting. Click the right arrow and move the Asset Type from _Current_ to _Available_. Add Asset Types by moving them to the Current section.

![Use the right and left arrows to add or remove asset types.](https://resources.learn.liferay.com/images/dxp/latest/en/using-search/search-pages-and-widgets/search-facets/type-facet/images/03.png)

Once finished, click the _Save_ button.
