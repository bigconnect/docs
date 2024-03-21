# Using Forks and Joins

Here you’ll learn how to use the _Fork_, _Join_ and _Join XOR_ nodes. These nodes are useful for workflow processes that have multiple reviewers like the Legal-Marketing Definition sample.

You can split the review task so that two reviews can happen at the same time. In the Legal-Marketing example workflow, the process is linear: the workflow goes from Marketing to Legal before publication, but you can use _Fork_ and _Join_ (or Join XOR) to build a concurrent workflow process.

### Using the Fork and Join Nodes <a href="#using-the-fork-and-join-nodes" id="using-the-fork-and-join-nodes"></a>

1. Go to the _Global Menu_ → _Applications_ → _Process Builder_.
2. Click the _Workflows_ tab.
3. Click the (![Add icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png)) to add a new workflow.
4. In the workflow designer Canvas, if there’s a transition connecting the Start node and End node, delete it: select it with your mouse and hit the Delete key on your keyboard.
5. Drag and drop the _Fork_ node onto the canvas then connect _Start_ node to the _Fork_ node. You can rename the nodes and connectors later.
6.  Drag and drop two _Task_ nodes. Connect the _Fork_ node to the two _Task_ nodes.

    ![Add the Fork and then the Task nodes.](https://resources.learn.liferay.com/images/dxp/latest/en/process-automation/workflow/designing-and-managing-workflows/workflow-designer/using-forks-and-joins/images/02.png)
7. Drag and drop the _Join_ node. Connect the two _Task Nodes_ to the _Join_ Node.
8. Connect the _Join_ node to the _End_ Node.
9.  Rename the nodes and connectors and add any Actions or Notifications.

    ![Publish the workflow when finished.](https://resources.learn.liferay.com/images/dxp/latest/en/process-automation/workflow/designing-and-managing-workflows/workflow-designer/using-forks-and-joins/images/01.png)
10. Click _Publish_ when finished.

#### Using the Join XOR Node <a href="#using-the-join-xor-node" id="using-the-join-xor-node"></a>

With a regular Join node, for the workflow to proceed beyond the join, the transition from both parallel executions must be invoked. However, if you use a Join XOR node instead, the workflow proceeds as long as the transition from one of the parallel executions is invoked.

\
