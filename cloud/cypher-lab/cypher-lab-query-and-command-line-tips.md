# Cypher Lab Query and Command-Line Tips

## Query Tips

You can remove all accumulated output frames with `:clear`. The ‘X’ button at the top right of each pane removes that frame and aborts a \(long-\)running statement. The maximum number of frames that are kept is configurable in the CypherLab Settings from the left-side menu.

If you want to review a past query, you can find the result pane and click the query above the graph visualisation to pull it back into the editor. The keyboard shortcuts listed above will help you work efficiently within the editor area.

You can also write and edit multi-line queries by switching to multi-line editing mode with `Shift+Enter`, then Enter will create newlines. You then need to run `Ctrl+Enter` or `Cmd+Enter` to run multi-line queries.

## Command Tips

`Ctrl+Up` and `Ctrl+Down` \(Mac users, use the Cmd key\) allows you to navigate command history, and you can access all command history with `:history`. The command history is persisted across CypherLab restarts.

You can put several Cypher statements into the command line input \(with semicolons to separate statements\) and run as a block by enabling the multi-statement query setting. To do this, go to the `Settings` tab on your CypherLab window and make sure the box is checked for `Enable multi statement query editor`. These steps are shown in the image below.

![](../../.gitbook/assets/image%20%28236%29.png)

## Output, Export, & Visualization

You can switch between `Graph`, `Table`, `Text`, and `Code` views to see the results in various formats by clicking the icons on the left of each pane.

{% hint style="info" %}
Don’t worry if you don’t see any output. You might just be in Graph mode, but had your query return tabular/scalar data. To see the results, just switch the mode to the Table view.
{% endhint %}

Query time is reported in the `Table` or `Code` views \(**don’t rely on that exact timing though**\), and it includes the latency and deserialization costs, not just the actual query execution time.

## CypherLab User Interface Guide

Goals

> This article demonstrates how to use the CypherLab for querying, visualization, and data interaction.

Beginner

For an introduction to Cypher Lab’s features and navigation, you can type `:play intro` into the command-line and press Enter or click the play button \(![cypher run button](https://s3.amazonaws.com/dev.assets.neo4j.com/wp-content/uploads/cypher_run_button.jpg)\).

### Built-in Guides

If you want to learn more about Cypher before diving in, click on any of the helpful links shown after startup \(commands also shown below\). 

To pin a particular pane to the top of the window \(under command line\) so that it stays at the top when you run queries after it, just click the pushpin button \(![pin button](https://s3.amazonaws.com/dev.assets.neo4j.com/wp-content/uploads/pin_button.png)\). This is especially helpful for pinning guides to the top so they do not get pushed down to the bottom of the window as you run queries.

| **Title** | **Description** | **Command** |
| :--- | :--- | :--- |
| Intro | A guided tour of Cypher Lab | `:play intro` |
| Cypher | Cypher graph query language introduction | `:play cypher` |
| The Movie Graph | A mini graph model of connections between actors and movies | `:play movie graph` |
| The Northwind Database | A classic use case of RDBMS to graph with import instructions and queries | `:play northwind graph` |

All you have to do is enter any of the above commands in the CypherLab command line \(at the top of the pane\) and follow the accompanying guides to walk through the training or scenarios. Guides are presented as slideshows that can be navigated by clicking the forward and back arrows in the pane to progress to the next or previous slide. To execute any of the queries provided within a CypherLab guide, simply click on the query, and it will populate in the command line. Then, just press Enter or the Play button to execute the statement! Results of the query will populate in a panel right below the command line.

### Styling CypherLab Visualization

As mentioned above, any queries you run in CypherLab will populate your results below the command line. Query results are rendered either as a visual graph, a table format, or an ascii-table result. You can switch between those with the icons on the left side of the result frame. If you cannot see the whole graph or the results display too close together, you can adjust by moving the visual view and dragging nodes to rearrange them. To move the view to see more parts of the graph, just click an empty spot within the graph pane and drag it. To rearrange nodes, you can click and drag them around.

The nodes will already have sensible captions assigned by the CypherLab, which auto-selects a property from the property list to use as a caption. You can see all the properties of that element if you click on any node or relationship. Properties will appear below the visualization. Larger property sets might be collapsed into a subset, but there is a little triangle on the right to unfold them.

For example, if you click on one of the `Movie` nodes in the MovieGraph \(`:play movie graph`\), then you can see its properties below the graph visual. The same applies for `Actor` nodes or the `ACTED_IN` relationships. If you click on any label or relationship **above** the graph visualization, you can then choose its styling in the area below the graph. Colors, sizes, and captions are selectable from there.

To see this for yourself, you can click on the `Person` label above the graph and change the color, size, and captions of all nodes labeled with `Person`. The first image below shows changes to nodes labeled `Person`. The second image shows changes to relationships labeled `ACTED_IN`.

![style person node](https://s3.amazonaws.com/dev.assets.neo4j.com/wp-content/uploads/style_person_node.png)

![style actedin relationship](https://s3.amazonaws.com/dev.assets.neo4j.com/wp-content/uploads/style_actedin_relationship.png)

### Useful Commands and Keyboard Shortcuts

| Shortcut | Purpose |
| :--- | :--- |
| `:help` | Help System |
| `:help commands` | Useful Commands |
| `:clear` | Clear Frames |
| `:style [reset]` | Styling Popup & Reset |
| `:help keys` | Keyboard Help |
| Ctrl+Enter or Cmd+Enter | Execute Statement |
| Ctrl+Up or Cmd+Up | Previous Statement |
| Shift+Enter | Enter Multiline Mode |
| / | Move Focus to Editor |
| ESC | Toggle Editor to Full Screen |

### Query and Command-Line Tips

Query Tips

You can remove all accumulated output frames with `:clear`. The ‘X’ button at the top right of each pane removes that frame and aborts a \(long-\)running statement. The maximum number of frames that are kept is configurable in the CypherLab Settings from the left-side menu.

If you want to review a past query, you can find the result pane and click the query above the graph visualisation to pull it back into the editor. The keyboard shortcuts listed above will help you work efficiently within the editor area.

You can also write and edit multi-line queries by switching to multi-line editing mode with Shift+Enter, then Enter will create newlines. You then need to run Ctrl+Enter or Cmd+Enter to run multi-line queries.Command Tips

Ctrl+Up and Ctrl+Down \(Mac users, use the Cmd key\) allows you to navigate command history, and you can access all command history with `:history`. The command history is persisted across CypherLab restarts.

You can put several Cypher statements into the command line input \(with semicolons to separate statements\) and run as a block by enabling the multi-statement query setting. To do this, go to the `Settings` tab on your CypherLab window and make sure the box is checked for `Enable multi statement query editor`. These steps are shown in the image below.

[![enable multiline queries](https://s3.amazonaws.com/dev.assets.neo4j.com/wp-content/uploads/enable_multiline_queries.jpg)](https://s3.amazonaws.com/dev.assets.neo4j.com/wp-content/uploads/enable_multiline_queries.jpg)Output, Export, & Visualization Tips

You can switch between `Graph`, `Table`, `Text`, and `Code` views to see the results in various formats by clicking the icons on the left of each pane.

|  | Don’t worry if you don’t see any output. You might just be in Graph mode, but had your query return tabular/scalar data. To see the results, just switch the mode to the Table view. |
| :--- | :--- |


Query time is reported in the `Table` or `Code` views \(**don’t rely on that exact timing though**\), and it includes the latency and deserialization costs, not just the actual query execution time.

You can also download the results as a CSV from the `Table`, `Text`, and `Code` views or download the graph as an SVG or PNG from the `Graph` view, as shown below.

![](../../.gitbook/assets/image%20%28597%29.png)

![](../../.gitbook/assets/image%20%28304%29.png)

## Graph Metadata

In the left-side menu, the top icon is the database section, where you can find the **currently used** node labels, relationship types, and property keys. Clicking on any one of those options runs a quick query to show you a sample of the graph with those elements.

## Setting Favorites

If you currently have an empty frame, you can display some nodes and relationships by using the **Favorites** in the left-side menu. Cypher Lab stores a few default favorites to get you started. Just click on the **Basic Queries**, then choose **Get Some Data** and run the query. This executes the statement `MATCH (n) RETURN n limit 100`, which fetches some nodes.

You can save your own queries as favorites by “starring” them. Just populate the CypherLab command line with the query you want to favorite, then click the **Favorites** icon to the right of the command line. This will add the query to your Favorites list in the left-side menu. To run one of your Favorites, click on the left-side menu Favorites, choose the query, and run it.

To provide a title or helpful info, you can use a comment `// comment` above your query to provide a title. The Favorites menu uses your comment as the query name.

Creating folders can help organize your favorites, and you can rearrange them by dragging or delete them if they are no longer useful.

## Advanced Styling

For more advanced styling, you can bring up the style-viewer with `:style` and copy/paste the graph-style-sheet \(GRASS\) that is returned. You can edit this stylesheet offline, save the file as a .grass file, and drag it back onto the drag-area of the viewer.

{% hint style="info" %}
You can reset to the default styles with **:style reset**
{% endhint %}

Within the GRASS file, you can change colors, fonts, sizes, outlines, and titles per node label and relationship type. It is also possible to combine multiple properties into a caption with `caption: '{name}, born in {born}';`

![](../../.gitbook/assets/image%20%28453%29.png)

## Configuration

The defaults for all the settings can adjusted at any time by going to the configuration option on the left-side menu. Some possible config changes and views are listed below.

* You can retrieve the current configuration with `:config`
* Individual settings are configured with the following defaults:
  * `:config maxNeighbours:100` – maxiumum number of neighbours for a node
  * `:config maxRows:100` – maximum number of rows for the tabular result



