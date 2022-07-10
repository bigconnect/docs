# Reviewing Assets

User interaction is required at each workflow process Task Node. Where do users complete tasks? In the _My Workflow Tasks_ application of the User menu.

![Figure 1: Users manage workflow tasks from their My Workflow Tasks widget.](https://liferay-support.zendesk.com/hc/article\_attachments/360029928892/workflow-myworkflow-tasks-menu.png)

### Asset Submission to Workflow <a href="#asset-submission-to-workflow" id="asset-submission-to-workflow"></a>

If an Asset has a workflow activated, when a user clicks Submit for Publication, the workflow definition determines the next step. A user assigned a Role associated to the workflow task receives a Notification indicating that there is a workflow task to complete.

**Workflow Assignees Have Implicit Asset Permissions:** Users with permission to execute a workflow task (e.g., Users with the Portal Content Reviewer Role) have full resource action permissions over the assets they can review. These permissions apply in the My Workflow Tasks widget in the User’s personal page and anywhere else actions on the Asset can be performed.

For example, consider a User with two permissions:

* The Portal Content Reviewer Role enables Users to review workflow submissions and grants edit and delete permissions on the content they’re reviewing.
* Users also have permission to view Web Content Articles in the Site’s _Content_ section.

Neither permission explicitly grants the User management permissions on Web Content Articles. Users cannot normally edit or delete a Web Content Article, for example. However, if a Web Content Article is sent to the workflow, Users can access the Web Content Article for review (in their _Assigned to Me_ or _Assigned to my Roles_ section of My Workflow Tasks), and they can edit or delete the content while reviewing it in the workflow. While it’s in the status _Pending_, they can also edit or delete the article from Site Administration → Content → Web Content because of their implicit permissions granted by the workflow system. This additional permission is temporary, and the normal resource permissions are activated once the Web Content Article exits the workflow process (for example, it’s rejected or approved).

![Figure 2: A User with VIEW permission on Web Content cannot manage Approved Articles.](https://liferay-support.zendesk.com/hc/article\_attachments/360029912691/workflow-approved-permissions.png)

![Figure 3: A User with access to Web Content in the Workflow can manage Pending Articles.](https://liferay-support.zendesk.com/hc/article\_attachments/360029912631/workflow-pending-permissions.png)

### Assigning the Task <a href="#assigning-the-task" id="assigning-the-task"></a>

Workflow Tasks can be completed only by certain users, based on the Assignment.

All workflow tasks assigned directly to a user are listed in the My Workflow Task widget’s _Assigned to Me_ tab.

![Figure 4: The assets assigned to a user are listed in Assigned to Me.](https://liferay-support.zendesk.com/hc/article\_attachments/360029912651/workflow-assigned-to-me.png)

If a workflow was assigned to a Role that the user occupies, the workflow’s tasks appear in the _Assigned to My Roles_ tab.

![Figure 5: The Assets assigned to Roles are listed in each associated users Assigned to My Roles tab.](https://liferay-support.zendesk.com/hc/article\_attachments/360029928872/workflow-assigned-to-my-roles.png)

To claim a task, the user must move the task into the _Assigned to Me_ tab.

1. Click the Asset’s _Actions_ button (![Actions](https://liferay-support.zendesk.com/hc/article\_attachments/360029928852/icon-actions.png)) and select _Assign to Me_.
2. Add a comment in the pop-up box if necessary, and click _Done_.

Alternatively, assign the task to another user.

1. Click the Asset’s _Actions_ button (![Actions](https://liferay-support.zendesk.com/hc/article\_attachments/360029928852/icon-actions.png)) and select _Assign to …_.
2. Select the user to assign the task.
3. Add a comment in the pop-up box if necessary, and click _Done_.

### Completing the Task <a href="#completing-the-task" id="completing-the-task"></a>

Once a task is assigned, it’s ready to be completed.

There’s a fast way to send an asset along in the review process:

1.  From the _Assigned to Me_ tab, click the _Actions_ button (![Actions](https://liferay-support.zendesk.com/hc/article\_attachments/360029928852/icon-actions.png)) and select _Approve_ or _Reject_.

    Note that options names in this menu are identical to the workflow definition’s [Transition](https://help.liferay.com/hc/en-us/articles/360028834712-Workflow-Definition-Nodes) names. Your menu might have different options than the _Approve_ and _Reject_ options in the figure below.
2. Enter a comment if desired and click _Done_.

![Figure 6: Complete Tasks right from the Assigned to Me list.](https://liferay-support.zendesk.com/hc/article\_attachments/360029912671/workflow-complete-task.png)

Here’s how to get a closer look at the Asset before sending it along in the workflow:

1.  From the _Assigned to Me_ tab, click the title of the Asset to review.

    The Task screen appears showing details about the Asset:

    <img src="https://liferay-support.zendesk.com/hc/article_attachments/360029928912/workflow-task-review.png" alt="Figure 7: Inspect Assets before completing the Task." data-size="original">

    Figure 7: Inspect Assets before completing the Task.
2. Inspect the Asset to your liking (or even edit it if you have permission) and click the _Actions_ button (![Actions](https://liferay-support.zendesk.com/hc/article\_attachments/360029928852/icon-actions.png)).
3. Send it along in the workflow by clicking one of the Transition names (for example, _Approve_ or _Reject_ in the Single Approver Definition).

And, you’re done. Once you’ve completed your workflow tasks, kick back and wait for more to come in.
