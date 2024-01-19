---
description: Add custom graph selection types
---

# Graph Selection

[Example code](https://github.com/mware-solutions/doc-examples/blob/master/extension-graph-selection)

Add custom [Cytoscape](http://js.cytoscape.org/) selection menu items. Graph provides select all, none, and invert by default.[![](https://camo.githubusercontent.com/f20e31eef4290b7b694a6cbdcd084bc211fb1d8ab7aa0fa7bf452800b596afa6/687474703a2f2f6c6f63616c686f73742f657874656e73696f6e2d706f696e74732f66726f6e742d656e642f677261706853656c6563746f722f73656c656374696f6e2e706e67)](https://camo.githubusercontent.com/f20e31eef4290b7b694a6cbdcd084bc211fb1d8ab7aa0fa7bf452800b596afa6/687474703a2f2f6c6f63616c686f73742f657874656e73696f6e2d706f696e74732f66726f6e742d656e642f677261706853656c6563746f722f73656c656374696f6e2e706e67)

This tutorial will create a selection menu item to select a random node or edge.

### Web Plugin

Register the plugin script in a web plugin.

```java
app.registerResourceBundle("/com/mware/examples/graph_selection/messages.properties");
```

[GraphSelectionWebAppPlugin.java \(line 17\)](https://github.com/mware-solutions/doc-examples/blob/master/extension-graph-selection/src/main/java/com/mware/examples/graph_selection/GraphSelectionWebAppPlugin.java#L17)

### Register Extension

Register the selection extension to find a random element and select it.

```javascript
require(['public/v1/api'], function(bc) {

    var doRandomSelection = function(cy) {
        var elements = cy.elements('.v,.e').unselect(),
            randomIndex = Math.floor(Math.random() * elements.length);
        elements[randomIndex].select();
    }
    doRandomSelection.identifier = 'com.mware-examples-graph-selection';
    doRandomSelection.visibility = 'always';

    bc.registry.registerExtension('org.bigconnect.graph.selection', doRandomSelection);
});
```

[plugin.js](https://github.com/mware-solutions/doc-examples/blob/master/extension-graph-selection/src/main/resources/com/mware/examples/graph_selection/plugin.js)

The `elements` function in Cytoscape will return all nodes and edges. This includes decorations on nodes, and temporary edges used for find path, etc. Filter by `.v,.e` to include only _real_ vertices and edges.

