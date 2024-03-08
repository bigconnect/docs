# About the Page Audit Tool

The Page Audit tool displays performance-related information about the server render time, component type, component’s origin, and cache status as well as warning messages about potential performance issues.

You need, at least, two [permissions](https://learn.liferay.com/web/guest/w/dxp/users-and-permissions/roles-and-permissions) to view and use the Page Audit tool. Configure them under Site and Asset Library Administration → Site Builder → Pages:

1. _Access in Site and Asset Library Administration_ under General Permissions
2. _Update_ permission under Pages

![The Page Audit is a powerful tool that helps users identify performance-related problems and solve them.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/page-performance-and-accessibility/about-the-page-audit-tool/images/01.gif)

### [Using the Page Audit Tool](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/page-performance-and-accessibility/about-the-page-audit-tool#using-the-page-audit-tool) <a href="#using-the-page-audit-tool" id="using-the-page-audit-tool"></a>

The Page Audit tool (![Page Audit tool](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-page-audit-tool.png)) appears on the applications bar at the top of the page.

1.  Click the _Page Audit_ tool icon to open a sidebar where the information is displayed on the right.

    Under the Performance tab appears a list of all components on the page.

    Each component has an information card showing the component’s name, render time (in milliseconds), type (a label that identifies the type: fragment, collection, or widget), and cache status along with icons to interact with the component.

    Under PageSpeed Insights, you can see specific recommendations for Liferay DXP if you have Google’s [PageSpeed Insights](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/page-performance-and-accessibility/analyze-seo-and-accessibility-on-pages) service configured (an API key is required).
2. Hover over the card to highlight the component in the page.
3. Click _Locate_ (![Locate icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-lens.png)) to find and center the component on the page.
4. With Fragments, click _Open_ (![Open in](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-open-in.png)) to navigate to the fragment library.
5. (Optional) Click on a card to display [warnings and basic information](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/page-performance-and-accessibility/about-the-page-audit-tool#warnings-and-basic-information) about the component.
6. (Optional) Use the [available tools](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/page-performance-and-accessibility/about-the-page-audit-tool#performance-tab-tools) (Search, Filter, and Sort) to organize or find components.

### [Performance Tab Tools](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/page-performance-and-accessibility/about-the-page-audit-tool#performance-tab-tools) <a href="#performance-tab-tools" id="performance-tab-tools"></a>

Under the Performance tab are tools to facilitate your navigation.

**Search:** Search for specific components by their names.

**Filter:** Filter by navigation (All or From Master - components from a Master page), cache status (Cached or Not Cached), or component type (e.g., Fragment, Widget, or Collection).

**Sort:** Sort components by render time. By default, items are ordered from slower to faster render time.

![Tools under the Performance tab help you organize and find components around the page.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/page-performance-and-accessibility/about-the-page-audit-tool/images/02.png)

### [Warnings and Basic Information](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/page-performance-and-accessibility/about-the-page-audit-tool#warnings-and-basic-information) <a href="#warnings-and-basic-information" id="warnings-and-basic-information"></a>

Click on a component card to display warnings and basic information about the component.

![Warnings indicate possible performance issues with page components.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/page-performance-and-accessibility/about-the-page-audit-tool/images/03.png)

#### [Warnings](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/page-performance-and-accessibility/about-the-page-audit-tool#warnings) <a href="#warnings" id="warnings"></a>

Warnings indicate possible performance issues with page components. It also includes suggestions to solve the problem and optimize your page.

Here’s is a list of warnings:

| Component                   | Warning                                                                                                                                                                   | Possible Solution                                                             |
| --------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------- |
| Collection Display Fragment | Pagination is disabled. This setting can affect page performance severely if the number of collection items is above 200. We strongly recommend using pagination instead. | Enable pagination for your collection display                                 |
| Image Fragment              | Big image file size used. Please, consider configuring adaptive media, lazy loading, or reducing the image size.                                                          | Configure adaptive media, enable lazy loading, or upload a smaller image file |

#### [Basic Information](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/page-performance-and-accessibility/about-the-page-audit-tool#basic-information) <a href="#basic-information" id="basic-information"></a>

The component card displays basic information about the component. Here is a list of details covered:

WARNING

Render time is an approximation and subject to slight variations depending on various factors (e.g., server status and load).

| Detail             | Description                                                                                       |
| ------------------ | ------------------------------------------------------------------------------------------------- |
| Server Render Time | The render time in milliseconds                                                                   |
| Component Type     | The component type: Fragment, Widget, or Collection                                               |
| Origin             | The origin of the component (e.g. This Page if it is originated in the same page it is displayed) |
| Cache Status       | The component’s cache status: Cached or Not Cached                                                |
