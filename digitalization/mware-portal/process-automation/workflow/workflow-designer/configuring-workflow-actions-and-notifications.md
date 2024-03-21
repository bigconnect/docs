# Configuring Workflow Actions and Notifications

Using the workflow designer, users can configure Workflow Actions and notifications for each node.

Actions are very adaptable and you can specify when the Action occurs: before entering the node, after exiting a node, or once a task node is assigned. For example, when a reviewer rejects a Web Content article, a Workflow Action sets the asset’s status as _Pending_ and automatically reassigns the article to the original author.

Workflow Notifications are sent to tell task assignees that the workflow needs attention or to update asset creators on the status of the process. They can be sent for tasks or any other type of node in the workflow.

![You can add actions and notifications to task nodes.](https://resources.learn.liferay.com/images/dxp/latest/en/process-automation/workflow/designing-and-managing-workflows/workflow-designer/configuring-workflow-actions-and-notifications/images/07.png)

However, not every node requires a Workflow Action or a notification. Generally, Start and End nodes do not have actions or notifications, unless you want to notify the original author that the review process has begun or ended.

In the Single Approver definition, the two Task nodes (Review and Update) have Actions and Notifications.

### Adding Actions <a href="#adding-actions" id="adding-actions"></a>

If you’re creating the Single Approver workflow definition, you must add a Reject _Action_ to the _Update_ Task node. This Reject Action contains a Groovy script which, when executed, sets the asset’s status as _denied_ then _pending_.

Follow the steps below:

1. Go to the _Global menu_ → _Applications_ → _Process Builder_.
2. Click the _Workflows_ tab.
3. Click on the Workflow definition (for example, _Single Approver_).
4.  Click the _Update_ node.

    ![Modify the Update node.](https://resources.learn.liferay.com/images/dxp/latest/en/process-automation/workflow/designing-and-managing-workflows/workflow-designer/configuring-workflow-actions-and-notifications/images/04.png)
5. Click an existing action or _New_ if there are none currently defined.
6. Enter _reject_ in the Name field.
7. Select _On Assignment_ from the _Execution Type_ dropdown menu.
8.  Enter the script in the _Template (Groovy)_ field (it’s called the _Script_ field in some versions). The Single Approver workflow contains an Update task with an action written in Groovy that sets the status of the asset as _denied_, then sets it to _pending_.

    ```java
    import com.liferay.portal.kernel.workflow.WorkflowConstants;
    import com.liferay.portal.kernel.workflow.WorkflowStatusManagerUtil;

    WorkflowStatusManagerUtil.updateStatus(WorkflowConstants.getLabelStatus("denied"), workflowContext);
    WorkflowStatusManagerUtil.updateStatus(WorkflowConstants.getLabelStatus("pending"), workflowContext);
    ```

    ![Add the reject action.](https://resources.learn.liferay.com/images/dxp/latest/en/process-automation/workflow/designing-and-managing-workflows/workflow-designer/configuring-workflow-actions-and-notifications/images/05.png)
9. Click the back arrow (![Back](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-angle-left.png)) when finished (in some versions you must click _Save_ in the action definition).

Next add a notification that informs the asset creator that the submission has been rejected at this time and needs more work.

### Adding Notifications <a href="#adding-notifications" id="adding-notifications"></a>

Add notifications to the _Update_ Task node which informs the asset creator that the submission needs more work and has been reassigned back to him.

Follow the steps below:

1. In the Update node’s properties sidebar, click an existing notification or _New_ if none are defined yet.

![Notifications are found in the Properties tab.](https://resources.learn.liferay.com/images/dxp/latest/en/process-automation/workflow/designing-and-managing-workflows/workflow-designer/configuring-workflow-actions-and-notifications/images/03.png)

1. Enter the following:
   * **Name:** Creator Modification Notification
   * **Description**: Enter a description for this notification
2. Select _Freemarker_ from the _Template Language_ dropdown menu.
3.  Enter a message in the _Template_ field:

    * `Your submission was rejected by ${userName}, please modify and resubmit.`

    See Workflow Notification Template Variables to learn about the injected template variables.
4. Select the Notification Type(s):
   * Email
   * User Notification
5. Select _On Assignment_ from the _Execution Type_ dropdown menu. The Notification is sent when someone is assigned to this task.
6.  Select a recipient type (_Asset Creator_).

    ![Add an Notification to the Update node which notifies the creator that the submission has been rejected.](https://resources.learn.liferay.com/images/dxp/latest/en/process-automation/workflow/designing-and-managing-workflows/workflow-designer/configuring-workflow-actions-and-notifications/images/06.png)
7. Click the back arrow (![Back](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-angle-left.png)) when finished (in some versions you must click _Save_ in the notification section).

Notifications on the Update node have been added.

\
