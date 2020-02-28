# Using the Graph

BigConnext Explorer has an advanced tool for graph visualization. You can create multiple individual visualisation graphs where you can place elements or create new ones, search how things are related and apply some graph algorithms and layouts.

{% hint style="warning" %}
The graph is not designed for displaying too many elements. 

We performed many tests and determined that 10,000 should be the maximum number of items that can be added to still have a decent experience. Of course this depends a lot of your desktop/laptop CPU power and the browser you use.
{% endhint %}

![Graph showing 15,000 elements with a directed layout](../../.gitbook/assets/image%20%28427%29.png)

The Graph can be accessed by going to **Analyze** in the top menu bar and then click on the **New** dropdown button and select **Graph**. This will create an empty graph.

![](../../.gitbook/assets/image%20%28353%29.png)

There are numerous ways to get data onto the graph:

* Copy & Paste search results from the search page
* Copy & Paste items from another Graph or Map
* Drag & Drop or UPLOAD one or more files from your computer
* Create a new element by clicking the CREATE card
* Load all elements from a saved search

{% hint style="info" %}
If you add load data from a dataset \(saved search\), you need to apply a layout after loading the data, because all loaded elements will be overlapped.
{% endhint %}

For this tutorial, we will use an existing graph that looks like this:

![](../../.gitbook/assets/image%20%28450%29.png)

This is a graph built in our Fraud Detection demo that is available online.

### Selecting items

There are three ways how you can select items on a graph:

* By clicking on the respective item \(node or relationship\)
* By holding the Shift key and clicking one or more items.
* By pressing Shift and drawing a rectangle to select items in an area. 

### Creating items

The graph is the main tool for creating new items: nodes and relationships.

#### Nodes

To create a new node, right-click in an empty area of the graph and choose **New Entity... \(**or press Alt+N\). A popup will be displayed asking the details of the new entity:

![](../../.gitbook/assets/image%20%28212%29.png)

Choose a concept and optionally provide a title, justification and security label. Once finished, click on **Create** and the new node will be created.

#### Relationships

To create a new relationship, right-click on a node and choose **Connect** from the context menu. An arrow with an empty circle will be displayed for you to select the target node. Select the target node and the following popup will be displayed:

![](../../.gitbook/assets/image%20%28539%29.png)

Choose the appropriate type of relationship to be created, an optional justification and security label. Once finished, click **Connect** to create the relationship.

### Deleting items

To delete items, just select one or more nodes and relations and press the **Delete** key.

{% hint style="warning" %}
Deleting a node/relationship from the Graph **DOES NOT delete it from the system**. It just removes it from the graph. You can always add it back.
{% endhint %}

### Layouts

To rearrange items on a graph, you must apply a layout. BigConnect Explorer provides some well known layout algorithms that can suit various purposes:

* Circle Breathfirst
* TopDown Breathfirst
* Dagre
* Concentric
* Circle
* Cola
* Grid
* Random
* D3

The layout menu ca be accessed by right-clicking in an empty area on the graph and choosing Layout. Once a layout is selected, it will be immediately applied to all elements on the graph.

Some example layouts applied on the graph displayed above:

![Circle](../../.gitbook/assets/image%20%28556%29.png)

![Concentric](../../.gitbook/assets/image%20%28390%29.png)

![TopDown Breathfirst](../../.gitbook/assets/image%20%28262%29.png)

![D3 Force Directed](../../.gitbook/assets/image%20%2855%29.png)

A layout can be applied on all elements or on the currently selected elements.

### Algorithms

BigConnect Explorer provides some algorithms that can influence the size of a node:

* [PageRank](https://ro.wikipedia.org/wiki/PageRank)
* [Degree Centrality](https://en.wikipedia.org/wiki/Centrality#Degree_centrality)
* [Closeness Centrality](https://en.wikipedia.org/wiki/Closeness_centrality)
* [Betweenness Centrality](https://en.wikipedia.org/wiki/Betweenness_centrality)

The can be accessed by right-clicking in an empty area of the graph and choosing the **Vertex Size** menu item. 

After applying PageRank on the graph, we can see that some nodes are bigger than others, because they have the greatest page rank score. 

![PageRank](../../.gitbook/assets/image%20%28377%29.png)

### Add Related items

This feature brings all directly connected nodes of one or more selected nodes. To enable it, select one or more nodes from the graph and then right click on one of them to bring up the context menu and select **Add Related**

![](../../.gitbook/assets/image%20%28288%29.png)

You can optionally filter out what related items to load, by choosing a specific concept and relationship. Click Add Related when done and the system will display all nodes that have a first degree connection with the selected nodes.

{% hint style="info" %}
You can also press Alt+R to show the Add Related popup.
{% endhint %}

### Finding connections

BigConnect Explorer allows you to see how two nodes are connected, up to the 5th degree. 

To do so, right-click on the source node and choose **Find Path** from the context menu. An arrow with an empty circle will be displayed for you to select the target node. Select the target node and the following popup will be displayed:

![](../../.gitbook/assets/image%20%28335%29.png)

Choose the appropriate options and click **Find Path**. A background job will be launched that will search for  nodes in the entire system. 

![](../../.gitbook/assets/image%20%2881%29.png)

Click on the **2 Paths** button and the system will show a preview of existing paths:

![](../../.gitbook/assets/image%20%28551%29.png)

Finally, click on the **ADD** button to add all nodes that lie on the found paths on the graph.

### Shortest path

The graph provides an option to visually highlight the shortest path between two selected nodes. 

To do so, select the right-click on the source node and choose **Show Path** from the context menu. An arrow with an empty circle will be displayed for you to select the target node. Select the target node and the system will highlight the shortest path between the two nodes:

![](../../.gitbook/assets/image%20%28435%29.png)

The shortest path is highlighted in light blue.

{% hint style="info" %}
The [A\* Search](https://en.wikipedia.org/wiki/A*_search_algorithm) algorithm is used to find the shortest path between the selected nodes.
{% endhint %}

### Grouping elements

The graph provides an option to merge multiple nodes into one. Select two or more nodes, right-click in an empty area of the graph and choose **Collapse**. The selected nodes and their relationships will be grouped together. 

To un-collapse the group, right-click on it and select **Uncollapse**.



