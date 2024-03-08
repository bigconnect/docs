# Content Dashboard Interface

The Content Dashboard user interface has three main sections:

1. [Content Audit Tool](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/content-dashboard/content-dashboard-interface#content-audit-tool)
2. [Contents List](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/content-dashboard/content-dashboard-interface#contents-list)
3. [Content Info Sidebar](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/content-dashboard/content-dashboard-interface#content-info-sidebar)

![The Content Dashboard user interface has three main sections.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/content-dashboard/content-dashboard-interface/images/01.png)

To access the dashboard, open the _Global Menu_ ( ![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png) ), go to the _Applications_ tab, and click _Content Dashboard_. Here you can view the [content audit tool](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/content-dashboard/content-dashboard-interface#content-audit-tool) (1) at the top of the page and the [contents list](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/content-dashboard/content-dashboard-interface#contents-list) (2) at the bottom. From the contents list, you can access the [content info sidebar](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/content-dashboard/content-dashboard-interface#content-info-sidebar) (3) by hovering over a content item and clicking the _Info_ button ( ![Info](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-information.png) ). Alternatively, you can click the content item’s _Actions_ button ( ![Actions Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png) ) and select _Info_.

NOTE

If the instance is connected to Analytics Cloud, you can also select _View Metrics_ to open the Content Performance side panel. See [About the Content Performance Tool](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/page-performance-and-accessibility/about-the-content-performance-tool) for more information.

### [Content Audit Tool](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/content-dashboard/content-dashboard-interface#content-audit-tool) <a href="#content-audit-tool" id="content-audit-tool"></a>

The content audit tool displays assets from the [contents list](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/content-dashboard/content-dashboard-interface#contents-list) section in a chart. This chart represents the total number of assets for the selected [vocabularies and categories](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/content-dashboard/content-dashboard-interface#customizing-vocabularies-and-categories-for-the-content-audit-tool) and can include items from all sites and asset libraries. However, the chart only displays data the current user can access. You can also restrict chart results using [contents list filters](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/content-dashboard/content-dashboard-interface#filtering-and-sorting-content-in-the-contents-list).

![The content audit tool displays assets from the contents list section in a chart.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/content-dashboard/content-dashboard-interface/images/02.png)

TIP

If the chart does not display the expected information, [verify your categories](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/tags-and-categories/defining-categories-and-vocabularies-for-content) for the selected vocabularies and ensure that [your content is categorized](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/tags-and-categories/organizing-content-with-categories-and-tags).

The content audit tool includes four UI elements:

1. The chart legend
2. X-axis categories for the first vocabulary
3. Y-axis categories for the second vocabulary
4. The bar chart

![The content audit tool includes these UI elements.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/content-dashboard/content-dashboard-interface/images/03.png)

#### [Customizing Vocabularies and Categories for the Content Audit Tool](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/content-dashboard/content-dashboard-interface#customizing-vocabularies-and-categories-for-the-content-audit-tool) <a href="#customizing-vocabularies-and-categories-for-the-content-audit-tool" id="customizing-vocabularies-and-categories-for-the-content-audit-tool"></a>

By default, the bar chart shows the total number of assets for each category in the selected vocabularies. To change these vocabularies,

1. Click the _Configure_ button ( ![Configure Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-settings.png) ) in the content audit area.
2.  Use the _left_ and _right_ arrow buttons to select which vocabularies to display in the chart.

    You can only use one or two vocabularies at a time. Also, you can only combine vocabularies from the same site. However, you can use global vocabularies with vocabularies from any site.
3.  Click the _up_ and _down_ arrow buttons to order the vocabularies.

    The chart uses the top vocabulary for the x-axis and the bottom vocabulary for the y-axis.

    ![Select vocabularies for the content audit chart.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/content-dashboard/content-dashboard-interface/images/04.png)
4. Click _Save_.

TIP

Click the _Flip Axes_ button ( ![Flip Axes](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-change.png) ) to quickly switch the x- and y-axes.

### [Contents List](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/content-dashboard/content-dashboard-interface#contents-list) <a href="#contents-list" id="contents-list"></a>

By default, the contents list displays all content you can access across your sites and asset libraries. From here, you can sort and filter this content using built-in or custom filters. The content audit tool chart changes dynamically based on the filters that you set.

The contents list includes four UI elements:

1. Filter and Sort selector
2. Filter editor
3. Search field
4. List of contents

![The contents list contains several elements to help you find content.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/content-dashboard/content-dashboard-interface/images/05.png)

In the list of contents are the following columns:

| Column                | Description                                                                                                                                                                                                                                |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Title                 | The content’s title.                                                                                                                                                                                                                       |
| Author                | The content’s author.                                                                                                                                                                                                                      |
| Type                  | The content type: web content article, document 7.4 U23+/GA23+                                                                                                                                                                             |
| Subtype               | The content [subtype](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/tags-and-categories/defining-categories-and-vocabularies-for-content#creating-subcategories).                                             |
| Site or Asset Library | The content item’s location in the Liferay instance.                                                                                                                                                                                       |
| Status                | The workflow statuses for each content item.                                                                                                                                                                                               |
| Vocabulary (x-axis)   | The categories applied to the content item from the content audit tool’s [x-axis vocabulary](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/content-dashboard/content-dashboard-interface#content-audit-tool). |
| Vocabulary (y-axis)   | The categories applied to the content item from the content audit tool’s [y-axis vocabulary](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/content-dashboard/content-dashboard-interface#content-audit-tool). |
| Modified Date         | The content’s last modified date                                                                                                                                                                                                           |
| Review Date           | The content’s review date                                                                                                                                                                                                                  |

The content list uses the _Page_ icon ( ![Page Icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-page.png) ) to identify assets that have a [display page template](https://learn.liferay.com/web/guest/w/dxp/site-building/displaying-content/using-display-page-templates).

You can hover over content in the list to access these actions:

* _View_ ( ![View](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-preview.png) ): View the content’s display page.
* _Edit_ ( ![Edit](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-edit.png) ): Open the content in edit mode.
* _Info_ ( ![Information](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-information.png) ): Open the [content info](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/content-dashboard/content-dashboard-interface#content-info-sidebar) sidebar.
*   _View Metrics_ ( ![View Metrics](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-analytics.png) ): Open the [content performance panel](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/page-performance-and-accessibility/about-the-content-performance-tool).

    ![Access the content info sidebar and content performance panel from the Actions menu or the icons on each content row.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/content-dashboard/content-dashboard-interface/images/06.png)

#### [Filtering and Sorting Content in the Contents List](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/content-dashboard/content-dashboard-interface#filtering-and-sorting-content-in-the-contents-list) <a href="#filtering-and-sorting-content-in-the-contents-list" id="filtering-and-sorting-content-in-the-contents-list"></a>

From the _Filter and Order_ drop-down menu, select a [pre-defined filter](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/content-dashboard/content-dashboard-interface#filter-options-reference). You can also sort content by title or modified date.

You can use the search bar to filter content by keyword or click a bar in the [content audit](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/content-dashboard/content-dashboard-interface#content-audit-tool) chart to filter content by the selected category.

![Use the filter editor to remove a specific filter or all of them from the filter area.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/content-dashboard/content-dashboard-interface/images/07.png)

See [Understanding Contents List Filters](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/content-dashboard/content-dashboard-interface#understanding-contents-list-filters) for more information on how combining filters works.

NOTE

When you close the dashboard page, the default filter and sorting option are reset.

#### [Filter Options Reference](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/content-dashboard/content-dashboard-interface#filter-options-reference) <a href="#filter-options-reference" id="filter-options-reference"></a>

For 7.4+

From the _Filter and Order_ drop-down menu, you can choose these options:

| Filter                | Description                                                                                                                                                                                                                                                                                                                                      |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Categories            | Filters using one or more [categories](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/tags-and-categories/organizing-content-with-categories-and-tags) from your vocabularies.                                                                                                                                       |
| Site or Asset Library | Filters by site or asset library.                                                                                                                                                                                                                                                                                                                |
| Type                  | Filters by content type and subtype, including out-of-the-box and custom subtypes.                                                                                                                                                                                                                                                               |
| Tags                  | Filters by content [tags](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/tags-and-categories/tagging-content-and-managing-tags).                                                                                                                                                                                     |
| Extension             | <p>Filters by file extension:<br></p><ul><li><code>audio</code></li><li><code>code</code></li><li><code>compressed</code></li><li><code>image</code></li><li><code>presentation</code></li><li><code>spreadsheet</code></li><li><code>text</code></li><li><code>vectorial</code></li><li><code>video</code></li><li><code>other</code></li></ul> |
| Filter by Author      | Filters by content author.                                                                                                                                                                                                                                                                                                                       |
| Filter by Status      | Filters by the content’s [workflow status](https://learn.liferay.com/web/guest/w/dxp/process-automation/workflow/introduction-to-workflow).                                                                                                                                                                                                      |
| Filter by Review Date | Filters by the content’s review date.                                                                                                                                                                                                                                                                                                            |
| Order By              | Filters by the content’s title, modified date, or review date.                                                                                                                                                                                                                                                                                   |

![Filter content items.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/content-dashboard/content-dashboard-interface/images/08.png)

#### [Understanding Contents List Filters](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/content-dashboard/content-dashboard-interface#understanding-contents-list-filters) <a href="#understanding-contents-list-filters" id="understanding-contents-list-filters"></a>

* When you use the categories or tags filters with more than one criteria, the results correspond to content that meets all the criteria (see example A.)
* When you choose multiple options for the same filter criteria, the contents list shows assets matching at least one option (see example B.)
* The criteria from different filters is combined using the `and` operator (see example C.)

| Example | Filter By           | Filter Example                                   | Contents List Results                                                       |
| ------- | ------------------- | ------------------------------------------------ | --------------------------------------------------------------------------- |
| A       | Categories          | Categories: _Entrepreneur_ and _Family_          | Only assets that belong to both the _Entrepreneur_ `and` Family Categories. |
| B       | Authors             | Authors: _Peter_ and _Linda_                     | Assets authored by _Peter_ `or` _Linda_ `or` both.                          |
| C       | Categories, Authors | Category is _Entrepreneur_ and Author is _Linda_ | Only assets under the _Entrepreneur_ Category `and` authored by Linda.      |

### [Content Info Sidebar](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/content-dashboard/content-dashboard-interface#content-info-sidebar) <a href="#content-info-sidebar" id="content-info-sidebar"></a>

The info sidebar provides access to actions and metadata for your selected asset. To open the sidebar, click the _Actions_ button ( ![Actions Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png) ) for the desired asset and select _Info_. Alternatively, you can hover over the asset and click the _Info_ button ( ![Information Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-information.png) ).

![Click the Info button to access the content info sidebar.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/content-dashboard/content-dashboard-interface/images/09.png)

This table lists available metadata and actions for different asset types.

| Asset Type                         | Metadata/Action                                                                                                                                                                                                              |
| ---------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Web Content                        | <ul><li>Title</li><li>Type 7.4 U53+/GA53+</li></ul>                                                                                                                                                                          |
| Documents and Media 7.4 U53+/GA53+ |                                                                                                                                                                                                                              |
| Blog Entries                       | <ul><li>Title</li><li>Type</li><li>Version (current active version)</li><li>Status</li><li>Author</li><li>Preview Image 7.4 U42+/GA42+</li><li>Display Date</li><li>Creation Date</li><li>Modified Date</li><li>ID</li></ul> |

![The content info sidebar provides metadata and actions for your content.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/content-dashboard/content-dashboard-interface/images/10.png)

#### [Version History](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/content-dashboard/content-dashboard-interface#version-history) <a href="#version-history" id="version-history"></a>

Available 7.4 U53+/GA53+

The Content Info panel displays the version history for assets from Web Content and Documents and Media. To view the version history, click _Info_ (![Info Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-information.png)) for a content item and go to the _Versions_ tab.

This tab lists up to 10 available versions for the item. If there are more versions, you can view them by clicking the View More button.

Each version displays the author of the version, the time of its creation, and a changelog. You can click the Actions button (![Actions Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) for a version to access these actions:

**Preview:** Show the rendered web content or document.

**Compare To:** Show the difference between the selected version and another version. This opens a new page that shows additions, deletions and formatting changes between versions. This action is available only when there are two or more active versions.

**Expire:** Deactivate a version and activate the next available version. For instance, if there are two versions, 1.2 and 1.1, and you expire 1.2, 1.1 becomes the active version.

![Click the Versions tab to view the version history for a content dashboard item.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/content-dashboard/content-dashboard-interface/images/11.png)
