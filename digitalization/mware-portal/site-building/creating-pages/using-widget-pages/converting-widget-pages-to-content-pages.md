# Converting Widget Pages to Content Pages

In older versions, widget pages provided exclusive features, such as custom layouts and customizable columns. Many of these exclusive features are available on content pages in 7.3 and above, so in most cases, you’ll want to create a content page.

If you’re upgrading to 7.3 or 7.4 from a previous version and migrating existing widget pages, you can convert them to content pages. You can convert a widget page to a content page right away, or you can preview the conversion, make changes, and then convert the page.

If you have multiple pages to convert, you can bulk convert them through the Site Administration, or you can use the available APIs and built-in script editor to convert all widget pages at once. All approaches are described here.

### Preview and Convert a Widget Page to a Content Page <a href="#preview-and-convert-a-widget-page-to-a-content-page" id="preview-and-convert-a-widget-page-to-a-content-page"></a>

To preview a conversion draft before converting your widget pages, follow these steps:

1. Open the _Site Menu_ (![Site Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu.png)) and navigate to _Site Builder_ → _Pages_.
2. Open _Actions_ (![Actions](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) next to the desired widget page and select _Convert to content page…_.
3. Acknowledge any warnings and make any required adjustments to the conversion draft. You can also add any fragments you’d like to the draft at this point.
4. Click _Publish_ to publish the preview draft, or click _Discard Conversion Draft_ to reset the widget page back to its original state. If there are warnings, a best-effort conversion, as described below, is completed.

#### Best Effort Conversions <a href="#best-effort-conversions" id="best-effort-conversions"></a>

Some features of widget pages aren’t supported by content pages and therefore can’t be converted exactly as they are. In these cases, the user is warned of any issues and a best-effort conversion is processed. These widget page features aren’t supported:

**Nested Applications:** Nested Applications are instead placed in the same column of the layout during the conversion. You may need to reorganize these applications after the best-effort conversion is complete.

**Customizable Sections:** If the page is customizable, any customizations made by the user are lost during conversion.

**Custom Page Layouts:** If the layout can be converted, the structure of the layout is conserved and the user is warned and given a chance to review the conversion draft before proceeding. If the layout can’t be converted, all widgets are placed in a single row and column and you must manually reorganize them after the page is converted.

If you’ve already confirmed that a custom layout template can be converted, you can disable the layout template conversion warning for the layout template so it doesn’t appear each time you convert a Widget Page that uses the layout.

Open the _Global Menu_ (![Applications Menu icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)), and navigate to _Control Panel_ → _Configuration_ → _System Settings_.

Select _Pages_ under Content and Data, navigate to _Widget Page to Content Page Converter_, and add the layout template ID to the list of Verified Layout Template IDs.

NOTE

While portlets are rendered according to render-weight on widget pages, that is not true for content pages. Portlets are rendered in the order they appear on the page on content pages (i.e. left to right, top to bottom), so you may notice some portlets are rendered sooner or later, depending on their placement on the content page.

### Converting Widget Pages Directly to Content Pages <a href="#converting-widget-pages-directly-to-content-pages" id="converting-widget-pages-directly-to-content-pages"></a>

Follow these steps to convert widget pages to content pages without a preview:

1. Open the _Site Menu_ (![Site Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu.png)) and navigate to _Site Builder_ → _Pages_.
2. Check the box for the widget page, or multiple widget pages. Click _Actions_ (![Actions Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) in the Management Toolbar and select the _Convert to Content Page_ option.

![You can convert multiple widget pages through the context menu](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/creating-pages/using-widget-pages/converting-widget-pages-to-content-pages/images/01.png)

1. Click _OK_ in the prompt that appears to complete the conversion.

### Bulk Converting All Widget Pages to Content Pages with APIs <a href="#bulk-converting-all-widget-pages-to-content-pages-with-apis" id="bulk-converting-all-widget-pages-to-content-pages-with-apis"></a>

You can bulk convert all widget pages on a site to content pages using the built-in script editor:

1. Open the _Global Menu_ (![Applications Menu icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)) and navigate to _Control Panel_ → _System_ → _Server Administration_ → _Script_.
2. Enter this script in the script window, making sure to replace the Group ID with your own. The Groovy script uses the `BulkLayoutConverter` interface to convert all widget pages with the given Group ID to content pages:

```java
import com.liferay.layout.util.BulkLayoutConverter
import com.liferay.portal.kernel.util.ArrayUtil
import org.osgi.framework.ServiceReference
import org.osgi.framework.BundleContext
import com.liferay.portal.kernel.module.util.SystemBundleUtil
import org.osgi.framework.BundleContext

BundleContext bundleContext = SystemBundleUtil.getBundleContext();

ServiceReference serviceReference = bundleContext.getServiceReference(BulkLayoutConverter.class.getName());

BulkLayoutConverter bulkLayoutConverter = bundleContext.getService(serviceReference);

long groupId = 20118L // Use your groupId

long[] plids = bulkLayoutConverter.getConvertibleLayoutPlids(groupId)

out.println("Convertible layouts before conversion:" + ArrayUtil.toStringArray(plids))

long[] convertedLayoutPlids = bulkLayoutConverter.convertLayouts(groupId)

out.println("Converted layouts:" + ArrayUtil.toStringArray(convertedLayoutPlids))

plids = bulkLayoutConverter.getConvertibleLayoutPlids(groupId)

out.println("Convertible layouts after conversion: " + ArrayUtil.toStringArray(plids))
```

1. Click _Execute_ to run the script.
2. The output should look similar to the snippet below to show that no convertible layouts (pages) remain:

```bash
Convertible layouts before conversion:[25, 26, 27]
Converted layouts:[25, 26, 27]
Convertible layouts after conversion: []
```

\
