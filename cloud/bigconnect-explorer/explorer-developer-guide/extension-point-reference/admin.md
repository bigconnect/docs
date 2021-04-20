---
description: Add a section to the admin page
---

# Admin

Admin extensions allow sections to be added in the admin pane that when clicked, open a custom component. This tutorial registers three admin extensions that show React, Flight, and an admin extension that opens a link.

[![](https://github.com/mware-solutions/bigconnect-docs/raw/master/.gitbook/assets/image%20%2835%29.png)](https://github.com/mware-solutions/bigconnect-docs/blob/master/.gitbook/assets/image%20%2835%29.png)

### Create a web plugin

First, create the web plugin that registers the resources.

```text
app.registerJavaScript("/com/mware/examples/admin/plugin.js");

app.registerJavaScriptComponent("/com/mware/examples/admin/React.jsx");
app.registerJavaScript("/com/mware/examples/admin/flight.js", false);
app.registerJavaScriptTemplate("/com/mware/examples/admin/flight-template.hbs");

app.registerResourceBundle("/com/mware/examples/admin/messages.properties");
```

### Register Extension

Register the admin extensions in the `plugin.js` file:

```text
 bc.registry.registerExtension('org.bigconnect.admin', {
  section: i18n('com.mware.examples.admin.section'),
  name: i18n('com.mware.examples.admin.react.name'),
  subtitle: i18n('com.mware.examples.admin.react.subtitle'),
  componentPath: 'com.mware/examples/admin/React'
});
```

The other two are very similar except we had `sortHint` to "Open URL" so it is first.

```text
options: {
    sortHint: 0
}
```

Notice we use the globally available `i18n` function to display strings. These are defined in `messages.properties`

```text
com.mware.examples.admin.section=Examples
com.mware.examples.admin.react.name=React
com.mware.examples.admin.react.subtitle=React admin example
```

