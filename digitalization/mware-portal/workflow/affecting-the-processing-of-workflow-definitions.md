# Affecting the Processing of Workflow Definitions

Workflow definitions all contain nodes: a Start Node, and End node, and at least one Task node. You might know that for the workflow to progress from one node to the other, you need Transitions. In this tutorial you’ll learn about using transitions to move the asset through the workflow from node to node, but you’ll also learn about some other features that affect the processing of the workflow.

* Transitions
* Forks
* Joins
* Conditions

Start by learning about the ever important Transition.

### Transitions <a href="#transitions" id="transitions"></a>

What are transitions? Workflow transitions connect one node to another. On exiting the first node, processing continues to the node pointed to by the transition. Every time you create an arrow from one node to another, Workflow Designer creates a transition.

![Figure 1: You connect nodes and direct workflow processing with transitions. The Single Approver workflow has transitions named Submit, Resubmit, Reject, and Approve.](https://liferay-support.zendesk.com/hc/article\_attachments/360030164071/workflow-designer-single-approver.png)

Each node you add has a pop-up menu letting you delete the node. As you hover your mouse over the edges of a node, notice your mouse pointer changes to a cross. The cross indicates you can connect the current node to another node. Hold down your mouse button and drag the mouse to start drawing your transition to another node. If you stop before reaching the edge of the next node, a pop-up displays node types you can create and connect to on-the-fly. To connect with an existing node, continue dragging the connector to that node.

When developing workflows in the Workflow Designer, make sure you go through all the transitions and name them appropriately. By default, these transitions get system generated names, so rename them to something more human-readable, as they’re displayed to workflow users as links that send the item to the next step in the workflow.

![Figure 2: In the Single Approver workflow, a user in the Review task can choose to Approve or Reject the asset, which sends the asset either to the EndNode or to the Update task.](https://liferay-support.zendesk.com/hc/article\_attachments/360030164091/workflow-designer-transition-link.png)

To rename transitions, click on the arrow representing the transition and use the Properties tab to set the name just like you do for a node.

### Forks and Joins <a href="#forks-and-joins" id="forks-and-joins"></a>

Sometimes you don’t need to wait for one task to be completed before moving on to another one. Instead, you want to do two or more things at the same time. To do this, transition to a fork node, make two transitions from the fork to your parallel tasks, and then come back together using a join node.

![Figure 3: Forks and Joins are used to enable parallel processing in the workflow.](https://liferay-support.zendesk.com/hc/article\_attachments/360030164111/workflow-designer-fork-join.png)

With a regular Join node, for the workflow to proceed beyond the join, the transition from both parallel executions must be invoked. However, if you use a Join XOR node instead, the workflow proceeds as long as the transition from one of the parallel executions is invoked.

Keep in mind that you must balance your fork and join nodes. In other words, for every fork, there must be a join that brings the parallel workflow threads back together.

### Conditions <a href="#conditions" id="conditions"></a>

Sometimes you must inspect an asset or its execution context, and depending on the result, send it to the appropriate transition. You need a node for a script that concludes by setting a value to one of your transitions.

![Figure 4: The Category Specific Approval definition starts with a Condition node.](https://liferay-support.zendesk.com/hc/article\_attachments/360030195892/workflow-designer-cat-specific-condition.png)

Now you’re equipped with the basic knowledge to design beautiful, effective workflows so that your assets can be properly reviewed before they’re published in your sites.
