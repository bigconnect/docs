---
description: Add custom styling to graph nodes
---

# Graph Node Class

[Example code](https://github.com/mware-solutions/doc-examples/blob/master/extension-graph-node-class)

Register a function that can add or remove classes from [Cytoscape](http://js.cytoscape.org/) nodes for custom styling.[![](https://camo.githubusercontent.com/b31a94ccc495bf1987cabe76ed89843049dbf328fcc36599094eae7ae3b75285/687474703a2f2f6c6f63616c686f73742f657874656e73696f6e2d706f696e74732f66726f6e742d656e642f67726170684e6f64652f636c6173732e706e67)](https://camo.githubusercontent.com/b31a94ccc495bf1987cabe76ed89843049dbf328fcc36599094eae7ae3b75285/687474703a2f2f6c6f63616c686f73742f657874656e73696f6e2d706f696e74732f66726f6e742d656e642f67726170684e6f64652f636c6173732e706e67)

### Web Plugin

Register the plugin script in a web plugin.

```java
app.registerJavaScript("/com/mware/examples/graph_node_class/plugin.js", true);
```

[GraphNodeClassWebAppPlugin.java (line 16)](https://github.com/mware-solutions/doc-examples/blob/master/extension-graph-node-class/src/main/java/com/mware/examples/graph\_node\_class/GraphNodeClassWebAppPlugin.java#L16)

### Register Extension

Register the class extension and apply a `unknownName` class when the [vertex](http://localhost/GLOSSARY.html#vertex) is a person with no name [property](http://localhost/GLOSSARY.html#property).

```javascript
bc.registry.registerExtension('org.bigconnect.graph.node.class', function(vertex, classes) {
    var name = _.findWhere(vertex.properties, { name: ONTOLOGY_CONSTANTS.PROP_TITLE })
    var concept = _.findWhere(vertex.properties, { name: ONTOLOGY_CONSTANTS.PROP_CONCEPT_TYPE })
    if (concept && concept.value === ONTOLOGY_CONSTANTS.CONCEPT_TYPE_PERSON && !name) {
        classes.push('unknownName')
    }
});
```

[plugin.js (lines 3–9)](https://github.com/mware-solutions/doc-examples/blob/master/extension-graph-node-class/src/main/resources/com/mware/examples/graph\_node\_class/plugin.js#L3-L9)

Register a style extension to test the behavior by adjusting the opacity.

```javascript
bc.registry.registerExtension('org.bigconnect.graph.style', function(cytoscapeStylesheet) {
     cytoscapeStylesheet.selector('node.unknownName')
         .style({
             'opacity': '0.75'
         })
});
```

[plugin.js (lines 11–16)](https://github.com/mware-solutions/doc-examples/blob/master/extension-graph-node-class/src/main/resources/com/mware/examples/graph\_node\_class/plugin.js#L11-L16)
