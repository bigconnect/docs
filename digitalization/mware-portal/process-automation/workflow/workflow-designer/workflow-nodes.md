# Workflow Nodes

A Workflow Node represents a specific point in an approval process, whether it begins the review process, approves or rejects the asset, or reassigns the task.

Below are the different types of nodes and the possible actions associated with each node.

### [Types of Nodes](https://learn.liferay.com/web/guest/w/dxp/process-automation/workflow/designing-and-managing-workflows/workflow-designer/workflow-nodes#types-of-nodes) <a href="#types-of-nodes" id="types-of-nodes"></a>

| Node          | Description                                                                                                                                           |
| ------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| Task          | _Task_ nodes indicate the workflow task and its assignee.                                                                                             |
| Fork and Join | _Fork_ and _Join_ are paired nodes for splitting the review process for multiple reviewers in parallel, and then rejoining when reviews are complete. |
| Join XOR      | The _Join XOR_ node allows the workflow to proceed as long as the transition from one of the parallel reviewers is invoked.                           |
| Condition     | The _Condition_ node establishes a condition before the review process can proceed.                                                                   |
| Start         | The _Start_ node begins the workflow.                                                                                                                 |
| End           | The default _End_ node by default sets the workflow status to _Approved_.                                                                             |
| State         | _State_ nodes place the review process in a particular mode, or state. Start and End nodes are special types of state nodes.                          |

#### [Start and End Nodes](https://learn.liferay.com/web/guest/w/dxp/process-automation/workflow/designing-and-managing-workflows/workflow-designer/workflow-nodes#start-and-end-nodes) <a href="#start-and-end-nodes" id="start-and-end-nodes"></a>

Start and end nodes kick off workflow processing and bring the asset to its final, approved state. Often you can use the default start and end nodes without modification.

End nodes have a default action that sets the workflow status to Approved using the Groovy scripting language:

```java
import com.liferay.portal.kernel.workflow.WorkflowStatusManagerUtil;
import com.liferay.portal.kernel.workflow.WorkflowConstants;

WorkflowStatusManagerUtil.updateStatus(WorkflowConstants.getLabelStatus("approved"), workflowContext);
```

#### [State Nodes](https://learn.liferay.com/web/guest/w/dxp/process-automation/workflow/designing-and-managing-workflows/workflow-designer/workflow-nodes#state-nodes) <a href="#state-nodes" id="state-nodes"></a>

State nodes can have [Actions and Notifications](https://learn.liferay.com/web/guest/w/dxp/process-automation/workflow/designing-and-managing-workflows/workflow-designer/configuring-workflow-actions-and-notifications). For example, you can create a node that sets the status to _Expired_, using this Groovy script:

```java
import com.liferay.portal.kernel.workflow.WorkflowStatusManagerUtil;
import com.liferay.portal.kernel.workflow.WorkflowConstants;

WorkflowStatusManagerUtil.updateStatus(WorkflowConstants.getLabelStatus("expired"), workflowContext);
```

#### [Condition Nodes](https://learn.liferay.com/web/guest/w/dxp/process-automation/workflow/designing-and-managing-workflows/workflow-designer/workflow-nodes#condition-nodes) <a href="#condition-nodes" id="condition-nodes"></a>

A _Condition_ node checks an asset or its execution context, and depending on the result, sends it to the appropriate transition. This node requires a script that sets a value to one of the transitions.

In the [Category Specific Definition](https://github.com/liferay/liferay-portal/blob/7.4.3.112-ga112/modules/apps/portal-workflow/portal-workflow-kaleo-runtime-impl/src/main/resources/META-INF/definitions/category-specific-workflow-definition.xml) is a script that looks up the asset in question, retrieves its [asset category](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/tags-and-categories/defining-categories-and-vocabularies-for-content), and sets an initial `returnValue`. Then it checks to see if the asset has been marked with the _legal_ category. If not it goes through _Content Review_ (the content-review task in the workflow), and if it does it goes through _Legal Review_ (the legal-review task in the workflow).

#### [Task Nodes](https://learn.liferay.com/web/guest/w/dxp/process-automation/workflow/designing-and-managing-workflows/workflow-designer/workflow-nodes#task-nodes) <a href="#task-nodes" id="task-nodes"></a>

_Task_ nodes represent where all the work is done. See [Creating Workflow Tasks](https://learn.liferay.com/web/guest/w/dxp/process-automation/workflow/designing-and-managing-workflows/workflow-designer/creating-workflow-tasks) and [Using Task Nodes](https://learn.liferay.com/web/guest/w/dxp/process-automation/workflow/designing-and-managing-workflows/workflow-designer/assigning-task-nodes).

\
