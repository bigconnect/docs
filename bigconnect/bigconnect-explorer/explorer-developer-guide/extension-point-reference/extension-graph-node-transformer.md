---
description: Adjust the data for graph nodes
---

# Graph Node Transformer

[Example code](https://github.com/mware-solutions/doc-examples/blob/master/extension-graph-node-transformer)

Allows extensions to adjust the data attribute of [Cytoscape](http://js.cytoscape.org/) nodes.[![](https://camo.githubusercontent.com/bd53cf167d28ecc530089bfe510ff3aa89760e151d59f60a01a15a989faae558/687474703a2f2f6c6f63616c686f73742f657874656e73696f6e2d706f696e74732f66726f6e742d656e642f67726170684e6f64652f7472616e73666f726d65722e706e67)](https://camo.githubusercontent.com/bd53cf167d28ecc530089bfe510ff3aa89760e151d59f60a01a15a989faae558/687474703a2f2f6c6f63616c686f73742f657874656e73696f6e2d706f696e74732f66726f6e742d656e642f67726170684e6f64652f7472616e73666f726d65722e706e67)

This tutorial will adjust the size of graph nodes based on how many properties they have.

### Web Plugin

Register the plugin script in a web plugin.

```java
app.registerJavaScript("/com/mware/examples/graph_node_transformer/plugin.js", true);
```

[GraphNodeTransformerWebAppPlugin.java (line 16)](https://github.com/mware-solutions/doc-examples/blob/master/extension-graph-node-transformer/src/main/java/com/mware/examples/graph\_node\_transformer/GraphNodeTransformerWebAppPlugin.java#L16)

### Register Extension

Register the transformer extension that just places a property count into the data object.

```javascript
bc.registry.registerExtension('org.bigconnect.graph.node.transformer', function(vertex, data) {
    data.numProperties = vertex.properties.length;
});
```

[plugin.js (lines 3–5)](https://github.com/mware-solutions/doc-examples/blob/master/extension-graph-node-transformer/src/main/resources/com/mware/examples/graph\_node\_transformer/plugin.js#L3-L5)

Register a style extension to test the transformer. The selector checks for the data property and adjusts the size of the node depending on `numProperties`.

```javascript
bc.registry.registerExtension('org.bigconnect.graph.style', function(cytoscapeStylesheet) {
    var pixelRatio = 'devicePixelRatio' in window ? window.devicePixelRatio : 1;
    cytoscapeStylesheet.selector('node[numProperties]')
        .style({
            'width': 'mapData(numProperties, 4, 15, ' + 30 * pixelRatio + ', ' + 75 * pixelRatio + ')',
            'height': 'mapData(numProperties, 4, 15, ' + 30 * pixelRatio + ', ' + 75 * pixelRatio + ')'
        })
});
```

[plugin.js (lines 7–14)](https://github.com/mware-solutions/doc-examples/blob/master/extension-graph-node-transformer/src/main/resources/com/mware/examples/graph\_node\_transformer/plugin.js#L7-L14)
