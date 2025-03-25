# Workflow Designer

The Process Builder’s workflow designer is a graphical interface for creating workflow process definitions. The convenient drag and drop interface makes workflow design easier than writing XML definitions by hand. It’s accessed in the Control Panel → Workflow → Process Builder.

<figure><img src="../../../../../.gitbook/assets/image (4) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

The workflow designer supports all workflow node types:

* Start and End nodes
* Fork and Join nodes
* Condition nodes
* State nodes
* Task nodes

In addition to the functionality provided by the drag and drop interface, you have the full power of Groovy (a Java-based scripting language) to perform programmatic actions on assets being moved through your workflows.

### Building Workflows <a href="#building-workflows" id="building-workflows"></a>

To build a workflow, navigate to the Global Menu (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)) → Control Panel → Process Builder.

Click the (![Add icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png)) to begin.

Each Workflow Node represents a specific point in an approval process, whether it starts the review process, approves or rejects the asset, or reassigns the task.

Workflow transitions link each node to create the desired flow in the review process. On exiting the first node, processing continues to the next node indicated by the transition.

### Localizing the Workflow <a href="#localizing-the-workflow" id="localizing-the-workflow"></a>

A workflow process’s text is experienced by Users in these ways:

* Administrators and workflow participants (e.g., workflow task assignees) see the workflow definition’s title in the list of workflows.
* Workflow participants (e.g., workflow task assignees) see the Node and Transition labels while managing their workflow content (e.g., in My Workflow Tasks).

The Workflow text that appears in the UI is translatable.

To translate the workflow,

1. Open a workflow or add a new one from the Global Menu → Applications → Process Builder.
2. If it’s a new workflow connect the start node to the end node with a [transition](https://learn.liferay.com/web/guest/w/dxp/process-automation/workflow/designing-and-managing-workflows/workflow-designer/workflow-transitions).
3.  All of the localizable fields were created in the instance’s default language. To begin translating, click the Language Selector near the workflow’s title field:

    ![Use the language selector to choose a translation language.](https://resources.learn.liferay.com/images/dxp/latest/en/process-automation/workflow/designing-and-managing-workflows/workflow-designer/workflow-designer-overview/images/02.png)
4. Select the language to begin translating.
5. Go through the workflow and translate the node labels, transition labels, and the workflow title.
6. Save the workflow.

Once the workflow definition is published and associated to an asset, participants like workflow task assignees will interact with the workflow in their language.
