# Using Layout Elements

Layout elements define drop zones for designing page or template layouts. By default, Liferay includes two layout elements: [Containers](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-fragments-and-widgets/using-fragments/using-layout-elements#containers) and [Grids](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-fragments-and-widgets/using-fragments/using-layout-elements#grids). Once added, you can drag and drop widgets or other fragments into these drop zones, including additional containers or grids to create even more complex layouts. After designing a layout, you can save layout compositions to reuse them in other pages and templates. See [Saving Fragment Compositions](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-fragments-and-widgets/using-fragments/saving-fragment-compositions) for more information.

![The Container and Grid fragments are for designing page layouts.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-fragments-and-widgets/using-fragments/using-layout-elements/images/01.png)

When you create a new page or template that supports fragments, a default drop zone appears. You can add any fragment to this area, but using the container fragment as the first element in your composition has important advantages:

* Greater layout control over page design, including flex display properties for [advanced compositions](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-fragments-and-widgets/using-fragments/saving-fragment-compositions#creating-advanced-compositions-with-the-container-fragment) (Liferay DXP 7.4+)
* [Save and reuse](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-fragments-and-widgets/using-fragments/saving-fragment-compositions) compositions quickly in other pages and templates
* Add a URL or page redirect to a fragment composition
* Leverage [style books](https://learn.liferay.com/web/guest/w/dxp/site-building/site-appearance/style-books/using-a-style-book-to-standardize-site-appearance) to standardize your site appearance

Once you’ve added a container, you can use the grid fragment to customize content layout for different viewports. See [Building Responsive Layouts with the Grid Fragment](https://learn.liferay.com/web/guest/w/dxp/site-building/optimizing-sites/building-a-responsive-site/building-responsive-layouts-with-the-grid-fragment) to learn more.

TIP

When dragging and dropping an element into a container or grid, Liferay highlights the drop zone to indicate where you’re placing it. Once added, you can drag and drop elements in the Browser side panel ( ![Browser](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-hierarchy.png) ) to rearrange them.

Finally, when you have a variety of nested elements on a page, a breadcrumb appears to assist you with traversing elements in a nested hierarchy.

![Use the breadcrumb system to navigate through the structure and select parent elements](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-fragments-and-widgets/using-fragments/using-layout-elements/images/04.png)

TIP

You can also select an element and use the shortcut `Shift + Enter` to select its parent.

### [Containers](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-fragments-and-widgets/using-fragments/using-layout-elements#containers) <a href="#containers" id="containers"></a>

> Previously called _Panels_

The container fragment adds a configurable drop zone to a content page. In addition to standard configuration options, you can configure the container’s HTML tag, display structure, and more. See [Configuring Fragments](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-fragments-and-widgets/using-fragments/configuring-fragments) for more information.

![Containers include both standard and unique configuration options.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-fragments-and-widgets/using-fragments/using-layout-elements/images/02.png)

### [Grids](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-fragments-and-widgets/using-fragments/using-layout-elements#grids) <a href="#grids" id="grids"></a>

> Previously called _Rows_

The grid fragment adds multiple drop zone modules to a page or template. These modules can be arranged horizontally and vertically. In addition to standard configuration options, you can determine the number of modules for each grid, the number of modules per row, and the vertical alignment of each module’s content. You can adjust the width of each module manually and add or remove padding between them. See [Configuring Fragments](https://learn.liferay.com/web/guest/w/dxp/site-building/creating-pages/page-fragments-and-widgets/using-fragments/configuring-fragments) for more information.

![Grids include standard and unique configuration options.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/page-fragments-and-widgets/using-fragments/using-layout-elements/images/03.png)
