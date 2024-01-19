# Managing Workflows with the Workflow Designer

The Workflow Designer gives you an intuitive interface for creating workflow definitions, from the simplest approval processes to the most complex business processes you can imagine. It features a drag and drop interface, workflow definition versioning, and a graphical representation of definitions and their nodes. Without the Workflow Designer, you’d have to hand-craft your entire workflow definition in XML. With the Workflow Designer, you might never need to look at a single line of XML. Of course, the Workflow Designer also lets you directly manipulate the XML (using the _Source_ tab) if you find it convenient.

### [Adding New Workflow Definitions with the Workflow Designer](https://help.liferay.com/hc/en-us/articles/360028821912-Managing-Workflows-with-the-Workflow-Designer#adding-new-workflow-definitions-with-the-workflow-designer) <a href="#adding-new-workflow-definitions-with-the-workflow-designer" id="adding-new-workflow-definitions-with-the-workflow-designer"></a>

Access the Workflow Designer by going to the Control Panel → Workflow → Process Builder. Click the Add icon (![Add](https://liferay-support.zendesk.com/hc/article\_attachments/360030195752/icon-add.png)).

Give the workflow definition a title and you’re ready to start designing your workflow.

![Figure 1: The Workflow Designers graphical interface makes designing workflows intuitive.](https://liferay-support.zendesk.com/hc/article\_attachments/360030195792/workflow-designer-canvas.png)

### Saving and Publishing Workflow Definitions <a href="#saving-and-publishing-workflow-definitions" id="saving-and-publishing-workflow-definitions"></a>

First, look below the canvas to see the buttons that let you _Save_ or _Publish_. Saving the definition as a draft lets you save your work so it’s not lost (due to a timeout, for example). It won’t be published (and assignable to assets), and it won’t be considered a version until the Publish button is clicked. Each time you save the workflow as a draft, a new revision is added to the Revision history. To see the Revision history and manage workflow versions, open the Info sidebar (![Information](https://liferay-support.zendesk.com/hc/article\_attachments/360030195712/icon-information.png)) and click _Revision History_.

![Figure 2: View a list of the current workflows that can be edited in the Workflow Designer.](https://liferay-support.zendesk.com/hc/article\_attachments/360030164011/workflow-designer-definitions.png)

### Adding Nodes <a href="#adding-nodes" id="adding-nodes"></a>

A new workflow is already populated with a start node, an end node, and a transition between them. To make the workflow the way you want it, add nodes to the workflow.

1. _Drag_ a node from the _Nodes_ palette on the right of the designer and _drop_ it on the canvas.
2. You’ll see it’s not connected to other nodes by a transition, so right now it can’t be used in the workflow. Delete the existing transition and then you can make new transitions to direct the _flow_ of your workflow (see more about transitions below if you’re not sure what they’re for or how to use them in the Workflow Designer).

Alternatively, start by deleting the default transition, then click the edge of the start node, drag a new transition from the start node to a blank spot on the canvas, and release it. You’re prompted to create a node at that spot, because you can’t have a transition without a starting point and an ending point on a node.

![Figure 3: You can add a node by creating a transition that ends at a blank spot on your Designer canvas.](https://liferay-support.zendesk.com/hc/article\_attachments/360030195732/workflow-designer-add-node.png)

That’s it. Of course, if you drag, say, a _Task_ node onto the canvas, it must be configured.

### Node Settings <a href="#node-settings" id="node-settings"></a>

Now you know how to add nodes to the workflow definition. By default you have three things added to your canvas: a start node, a transition, and an end node. Think of the _EndNode_ as the point in the workflow where an asset reaches the _Approved_ status. The _StartNode_ is where the asset goes from the _Draft_ status to _Pending_. You might decide to name your nodes to reflect what’s happening in each one. To name a node, double click it, and its _Settings_ appear. Then double click the value of the _Name_ property and you can edit the name. Click _Save_ when you’re done.

![Figure 4: You can edit a nodes settings.](https://liferay-support.zendesk.com/hc/article\_attachments/360030195772/workflow-designer-node-settings.png)

Of course, there’s more you can do besides changing node names. Actions, Notifications, and Assignments can be used to make your workflow definition useful and interactive. Keep reading to learn about these features.

\
