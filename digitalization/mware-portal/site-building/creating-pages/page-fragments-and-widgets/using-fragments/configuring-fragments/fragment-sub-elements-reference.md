# Fragment Sub-Elements Reference

In addition to General and Style settings, many fragments include sub-elements with their own configuration options. To access these options, begin editing a page or template ( ![Edit Icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-edit.png) ), click the desired fragment, and select one of its sub-elements in the Browser ( ![Browser Icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-hierarchy.png) ) sidebar menu.

![Many fragments include sub-elements with their own configuration options.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-fragments-and-widgets/using-fragments/configuring-fragments/fragment-sub-elements-reference/images/01.png)

Available options depend on the sub-element’s type and are organized into tabs: Mapping, Image Source, and Link.

| Sub-Element Type                                                                                                            | Mapping Tab | Image Source Tab | Link Tab |
| --------------------------------------------------------------------------------------------------------------------------- | ----------- | ---------------- | -------- |
| Simple Text Element ( ![Simple Text Icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-text2.png) ) | ✔           |                  | ✔        |
| Rich Text Element ( ![Rich Text Icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-text.png) )      | ✔           |                  |          |
| Link Element ( ![Link Icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-link.png) )                | ✔           |                  | ✔        |
| HTML Element ( ![HTML Icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-code.png) )                | ✔           |                  |          |
| Image Element ( ![Image Icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-picture.png) )           |             | ✔                | ✔        |

### Mapping Settings <a href="#mapping-settings" id="mapping-settings"></a>

Map the selected element to a field in an existing asset.

| Field | Description                                                                                                                  |
| ----- | ---------------------------------------------------------------------------------------------------------------------------- |
| Item  | Select an asset from Documents and Media, Web Content, Blogs, or Categories. The selected asset provides fields for mapping. |
| Field | Map the element to supported fields from the selected item.                                                                  |

NOTE

Supported fields include: Text, Select from List, Single Selection, Multiple Selection, Date, Date and Time, Numeric, Boolean, Link to Page, and Rich Text.

![Access a sub-element's Mapping settings in the Browser panel of the editing sidebar menu.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-fragments-and-widgets/using-fragments/configuring-fragments/fragment-sub-elements-reference/images/02.png)

### Image Source Settings <a href="#image-source-settings" id="image-source-settings"></a>

Configure the image displayed in the selected image element.

WARNING

Fragments cannot display images stored on Content Delivery Network (CDN) hosts. Do not reference images served through CDN hosts in your fragments.

| Field                            | Description                                                                                                                                                                                                                                                                                                                                        |
| -------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Source Selection                 | Select whether the image element uses _Direct_ or _Mapping_ for its source. When using Direct, you can select an image from Documents and Media or enter an image URL. When using Mapping, you can select an asset from Documents and Media, Web Content, Blogs, or Categories, and then map the image element to a field from the selected asset. |
| Image (_for Direct_)             | Select the desired image from the Document Library or enter an image URL.                                                                                                                                                                                                                                                                          |
| Resolution (_for Direct_)        | Select a resolution for the image. By default, the portal includes the following options: Auto, Thumbnail (300px), or Preview (1000px). However, you can use Adaptive Media to generate alternative resolutions.                                                                                                                                   |
| Image Description (_for Direct_) | Enter a localizable image description. To set a description for other languages, first switch the language for the page or template via the editing bar. Then, enter the desired text. See Localizing Fragment Configuration Fields for more information.                                                                                          |
| Item (_for Mapping_)             | Select an asset from Documents and Media, Web Content, Blogs, or Categories. The selected asset provides fields for mapping.                                                                                                                                                                                                                       |
| Field (_for Mapping_)            | Map the image element to the desired field from the selected item.                                                                                                                                                                                                                                                                                 |
| Enable Lazy Loading              | Enable lazy loading to render the image only when needed. While enabled, off screen images are not loaded during the page’s initial rendering. This can shorten the critical rendering path and improve page load times. By default, lazy loading is disabled.                                                                                     |

![Access a sub-element's Image Source settings in the Browser panel of the editing sidebar menu.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-fragments-and-widgets/using-fragments/configuring-fragments/fragment-sub-elements-reference/images/03.png)

### [Link Settings](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-fragments-and-widgets/using-fragments/configuring-fragments/fragment-sub-elements-reference#link-settings) <a href="#link-settings" id="link-settings"></a>

Set a URL, page reference, or asset mapping to add a clickable link to the selected element.

| Field                    | Description                                                                                                                                                                                                                                        |
| ------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Link                     | Determine whether the field uses a _URL_, _Page_, or _Mapped URL_.                                                                                                                                                                                 |
| URL (_for URL_)          | Enter a localizable URL for the element. To set a URL for other languages, first switch the language for the page or template via the editing bar. Then, enter the desired URL. See Localizing Fragment Configuration Fields for more information. |
| Page (_for Page_)        | Select a portal page for the element.                                                                                                                                                                                                              |
| Item (_for Mapped URL_)  | Select an asset from Documents and Media, Web Content, Blogs, or Categories. The selected asset provides fields for mapping.                                                                                                                       |
| Field (_for Mapped URL_) | Map the element to an available supported URL field from the item. Selecting a valid field automatically populates the URL field.                                                                                                                  |
| URL (_for Mapped URL_)   | This field is auto-populated by the mapped URL field.                                                                                                                                                                                              |
| Open in New Tab          | Determine whether clicking the link opens a new tab.                                                                                                                                                                                               |

NOTE

You can map the following elements to fields (_for Mapped URL_): Text, Select from List, Single Selection, Multiple Selection, Numeric, Rich Text, and Link to Page.

![Access a sub-element's Link settings in the Browser panel of the editing sidebar menu.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-fragments-and-widgets/using-fragments/configuring-fragments/fragment-sub-elements-reference/images/04.png)
