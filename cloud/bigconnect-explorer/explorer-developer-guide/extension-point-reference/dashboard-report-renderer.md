---
description: Add new types of charts
---

# Dashboard Report Renderer

[Example code](https://github.com/mware-solutions/doc-examples/blob/master/extension-dashboard-reportrenderer)

Adds additional output types for dashboard items that define a `report` or `item.configuration.report`

[![](https://github.com/mware-solutions/bigconnect-docs/raw/master/.gitbook/assets/image%20%2866%29.png)](https://github.com/mware-solutions/bigconnect-docs/blob/master/.gitbook/assets/image%20%2866%29.png)

For this tutorial, we'll create a new JSON renderer that simply takes the result, formats it, then prints it.

[![](https://github.com/mware-solutions/bigconnect-docs/raw/master/.gitbook/assets/image%20%2829%29.png)](https://github.com/mware-solutions/bigconnect-docs/blob/master/.gitbook/assets/image%20%2829%29.png)

### Create the Web Plugin

Register the resources to define the extension and the referenced component:

```java
app.registerJavaScript("/com/mware/examples/dashboard_reportrenderer/plugin.js", true);
app.registerJavaScript("/com/mware/examples/dashboard_reportrenderer/renderer.js", false);
app.registerResourceBundle("/com/mware/examples/dashboard_reportrenderer/messages.properties");
```

[DashboardReportrendererWebAppPlugin.java \(lines 16–18\)](https://github.com/mware-solutions/doc-examples/blob/master/extension-dashboard-reportrenderer/src/main/java/com/mware/examples/dashboard_reportrenderer/DashboardReportrendererWebAppPlugin.java#L16-L18)

### Register the Extension

Register the new report renderer. It will accept any response.

```javascript
bc.registry.registerExtension('org.bigconnect.web.dashboard.reportrenderer', {
    identifier: 'com.mware-examples-dashboard-reportrenderer',
    label: i18n('com.mware.examples.dashboard_reportrenderer.label'),
    supportsResponse: function(data) {
        return true;
    },
    componentPath: 'com/mware/examples/dashboard_reportrenderer/renderer'
});
```

[plugin.js \(lines 3–10\)](https://github.com/mware-solutions/doc-examples/blob/master/extension-dashboard-reportrenderer/src/main/resources/com/mware/examples/dashboard_reportrenderer/plugin.js#L3-L10)

### Define the Renderer

Create the renderer component and include the mixin.

```javascript
define([
    'public/v1/api',
    'dashboard/reportRenderers/withRenderer'
], function(api, withReportRenderer) {

    return api.defineComponent(JSONStringRenderer, withReportRenderer)

    function JSONStringRenderer() {
```

[renderer.js \(lines 1–8\)](https://github.com/mware-solutions/doc-examples/blob/master/extension-dashboard-reportrenderer/src/main/resources/com/mware/examples/dashboard_reportrenderer/renderer.js#L1-L8)

Now, implement the `processData` and `render` functions

```javascript
this.processData = function(data) {
    return JSON.stringify(data, null, 2);
}

this.render = function(d3, node, data) {
    d3.select(node)
        .style({
            'white-space': 'pre',
            'font-family': 'menlo',
            'font-size': '8pt'
        })
        .text(data);
}
```

[renderer.js \(lines 11–23\)](https://github.com/mware-solutions/doc-examples/blob/master/extension-dashboard-reportrenderer/src/main/resources/com/mware/examples/dashboard_reportrenderer/renderer.js#L11-L23)

### Renderer Mixin

The custom report renderer provides:

* Automatically requesting data using endpoint configuration
* Handling refresh and reflow events
* Basic click handling if aggregations found
* Error handling

If the renderer uses the mixin, the only function required is `render`. Optionally, a `processData` function can be defined to transform the raw server results. It's better to process the data in `processData`function instead of `render` because it will run once on `refreshData` events, instead of on every `reflow` event.

### Built-In Report Renderers

[![](https://camo.githubusercontent.com/fb32a1dea911f3c4a8a83eed71f7205dd833def64abb61f8ad7d420715e4c414/687474703a2f2f6c6f63616c686f73742f657874656e73696f6e2d706f696e74732f66726f6e742d656e642f64617368626f6172642f72656e64657265722d6261722d682e706e67)](https://camo.githubusercontent.com/fb32a1dea911f3c4a8a83eed71f7205dd833def64abb61f8ad7d420715e4c414/687474703a2f2f6c6f63616c686f73742f657874656e73696f6e2d706f696e74732f66726f6e742d656e642f64617368626f6172642f72656e64657265722d6261722d682e706e67)`org-bigconnect-bar-horizontal`

Horizontal bar chart, also supports stacked bars if two aggregations provided.[![](https://camo.githubusercontent.com/4a25134d9ce9ada902bcea9247dea28c46e2749fb5f6fa6bd295ccd0c9f3c4d8/687474703a2f2f6c6f63616c686f73742f657874656e73696f6e2d706f696e74732f66726f6e742d656e642f64617368626f6172642f72656e64657265722d6261722d762e706e67)](https://camo.githubusercontent.com/4a25134d9ce9ada902bcea9247dea28c46e2749fb5f6fa6bd295ccd0c9f3c4d8/687474703a2f2f6c6f63616c686f73742f657874656e73696f6e2d706f696e74732f66726f6e742d656e642f64617368626f6172642f72656e64657265722d6261722d762e706e67)`org-bigconnect-bar-vertical`

Vertical bar chart, also supports stacked bars if two aggregations provided.[![](https://camo.githubusercontent.com/63e1e91e9adf4f2e239fec9c00e90caa6c6844eeafdd6dfd35eaec2ae4d8961e/687474703a2f2f6c6f63616c686f73742f657874656e73696f6e2d706f696e74732f66726f6e742d656e642f64617368626f6172642f72656e64657265722d7069652e706e67)](https://camo.githubusercontent.com/63e1e91e9adf4f2e239fec9c00e90caa6c6844eeafdd6dfd35eaec2ae4d8961e/687474703a2f2f6c6f63616c686f73742f657874656e73696f6e2d706f696e74732f66726f6e742d656e642f64617368626f6172642f72656e64657265722d7069652e706e67)`org-bigconnect-pie`

A classic pie chart.[![](https://camo.githubusercontent.com/f181cae8d04ff34bb23a8fe983e005998ed621ed8b59ba90f29ce164dfd2a26c/687474703a2f2f6c6f63616c686f73742f657874656e73696f6e2d706f696e74732f66726f6e742d656e642f64617368626f6172642f72656e64657265722d746578742d6f766572766965772e706e67)](https://camo.githubusercontent.com/f181cae8d04ff34bb23a8fe983e005998ed621ed8b59ba90f29ce164dfd2a26c/687474703a2f2f6c6f63616c686f73742f657874656e73696f6e2d706f696e74732f66726f6e742d656e642f64617368626f6172642f72656e64657265722d746578742d6f766572766965772e706e67)`org-bigconnect-text-overview`

Text cards that show number and text.[![](https://camo.githubusercontent.com/538e34179034fcc1970f61840b70d7bd8520ef4dc8c04375b7faa9a795a95c4a/687474703a2f2f6c6f63616c686f73742f657874656e73696f6e2d706f696e74732f66726f6e742d656e642f64617368626f6172642f72656e64657265722d656c656d656e742d6c6973742e706e67)](https://camo.githubusercontent.com/538e34179034fcc1970f61840b70d7bd8520ef4dc8c04375b7faa9a795a95c4a/687474703a2f2f6c6f63616c686f73742f657874656e73696f6e2d706f696e74732f66726f6e742d656e642f64617368626f6172642f72656e64657265722d656c656d656e742d6c6973742e706e67)`org-bigconnect-element-list`

Standard list of elements, used in search results.

