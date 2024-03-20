# Configuring the Asset Publisher Display Settings

When using an Asset Publisher widget, you can configure several display settings to specify how users view your content.

1. Create a new page or edit an existing one
2. While editing the page, look at the Fragments and Widgets tab on the left of the page editor and search for the _Asset Publisher_ widget.
3. Drag and drop the _Asset Publisher_ widget into the editing area.
4. Select your assets.
5. Hover over the Asset Publisher and click _Options_ (![Options icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-app-options.png)) in the widget’s menu → _Configuration_.
6. Follow the Asset Publisher Display Configuration Reference and configure your widget.
7. Click _Save_ to apply your display settings.

Now, users can view the Asset Publisher widget displayed in the page as you configured it.

### Asset Publisher Display Configuration Reference <a href="#asset-publisher-display-configuration-reference" id="asset-publisher-display-configuration-reference"></a>

#### Display Settings <a href="#display-settings" id="display-settings"></a>

These templates are in every site by default:

| Display Template | Scope    | Description                                                                                                                                                                              |
| ---------------- | -------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Abstracts        | instance | Displays the first 200-500 characters of the content, defined by the **Abstract Length** field. This is the default display template.                                                    |
| Table            | instance | Displays the content in an HTML table which can be styled by a theme developer                                                                                                           |
| Title List       | instance | Displays the content’s title as defined by the user who entered it                                                                                                                       |
| Full Content     | instance | Displays the entire content of the entry                                                                                                                                                 |
| Map              | global   | displays geo-localized assets in either a Google Map or an Open Street Map provider. The map provider can be configured in Instance Settings, and Site Settings in the Advanced section. |
| Rich Summary     | global   | Displays a summary view of each asset along with a Read More link to the article’s full content                                                                                          |

These are the remaining settings for the Display Settings section:

| Setting                    | Description                                                                                                                                                                                                                                                                                                                                                                                 |
| -------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Abstract Length            | Select the number of characters to display for abstracts. The default is `200`. Note this option is only available for the Abstracts display template.                                                                                                                                                                                                                                      |
| Asset Link Behavior        | The default value is View On New Page, which looks for a page where the asset is being displayed (e.g. Content Display widget, Display Page Template, or an Asset Publisher which manually selects the asset). The Show Full Content value displays the full asset in the current Asset Publisher.                                                                                          |
| Number of Items to Display | The maximum number of assets that can be displayed. If pagination is enabled, this number represents the maximum number of assets that can be displayed per page. If no value is given, the default value is `20`. It is recommended to set the number of assets below `200` to improve performance.                                                                                        |
| Pagination Type            | The type of UI to display for pagination. Three options are available: _None_ displays no pagination controls; _Simple_ adds Previous and Next buttons for browsing through pages of assets; _Regular_ adds more options and information including First and Last buttons, a drop-down selector for pages, the number of items per page, and the total number of results (assets displayed) |

NOTE

If two Asset Publishers are on the same page, and one is configured with the Dynamic Asset Selection and View on New Page, and the other with the Manual Asset Selection, clicking on the dynamic Asset Publisher directs users to the manual Asset Publisher, where the asset is being displayed. As a result, users are redirected to the same page.

However, when there is only one dynamic Asset Publisher with the View on New Page configuration and the asset is not being displayed anywhere, the Asset Link Behavior changes to Show Full Content, which displays the full asset in the Asset Publisher widget itself.

![Several display settings are available for the Asset Publisher.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/using-the-asset-publisher-widget/configuring-asset-publisher-display-settings/images/01.png)

#### Set and Enable Options <a href="#set-and-enable-options" id="set-and-enable-options"></a>

Many of these options, such as printing, flags, ratings, comments, comment ratings, and social bookmarks work the same way they do in other applications.

| Setting                                                                   | Description                                                                                                                                                                                                                                                                                                                                                           |
| ------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Show Add Content Button                                                   | When selected, an Add New button appears, and users can add new assets directly from the Asset Publisher application. This is checked by default.                                                                                                                                                                                                                     |
| Show Metadata Descriptions                                                | Enables metadata descriptions such as Content Related to… or Content with tag… to display with the published assets.                                                                                                                                                                                                                                                  |
| Show Available Locales                                                    | Since content can be localized, you can have different versions of it based on locale. Enabling this option shows the available locales, so users can view the content in their languages.                                                                                                                                                                            |
| Set as the Default Asset Publisher for This Page                          | The Asset Publisher app is an instanceable app: multiple Asset Publishers can be added to a page and each has an independent configuration. The default Asset Publisher for a page is the one used to display content associated with the page. This setting is not available for Asset Publisher in display page template.                                           |
| Show only assets with asset publisher widget as its display page template | Display assets that only exist for the specified display page template.                                                                                                                                                                                                                                                                                               |
| Include tags specified in the URL                                         | Incorporate tags specified in the URL                                                                                                                                                                                                                                                                                                                                 |
| Enable …                                                                  | Enable/disable these options for displayed assets. The Print option adds a Print link to the full view of an asset. Clicking _Print_ opens a new browser window with a print view of the asset. Enabling flags, related assets, ratings, comments, comment ratings, or social bookmarks add links to the corresponding social features to the full view of the asset. |
| Social Bookmarks                                                          | Specify which social media links to display by moving the bookmark from the Available box to the Current box                                                                                                                                                                                                                                                          |
| Display Style                                                             | Specify one of two display styles for the selected social media links. _Inline_ shows icons for the selected sites in a line. _Menu_ shows a Share button with a drop-down menu containing selected sites as options.                                                                                                                                                 |

![There are several options that you can enable.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/using-the-asset-publisher-widget/configuring-asset-publisher-display-settings/images/02.png)

TIP

An alternate way to add flags, comments, and ratings to a page is through the Page Flags, Page Comments, and Page Ratings applications. Add the applications in the appropriate location near the asset that should have feedback.

#### Metadata <a href="#metadata" id="metadata"></a>

This section specifies what metadata to display with the asset entry. Move metadata from the Available box to the Current box to include it.

![You can display several different types of metadata with the asset.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/using-the-asset-publisher-widget/configuring-asset-publisher-display-settings/images/03.png)

#### Grouping <a href="#grouping" id="grouping"></a>

The Grouping section specifies dividing the displayed assets into groups. You can group assets by type or by vocabulary.

![You can separate assets into different groups.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/using-the-asset-publisher-widget/configuring-asset-publisher-display-settings/images/04.png)

For example, suppose you have a vocabulary called Membership Type with two categories: Premium and Regular. If you group assets by Membership Type, all assets with the Premium category appear in one group and all assets with the Regular category appear in another group. Grouping rules are applied before any ordering rules. The ordering rules are applied separately to each group of assets.

\
