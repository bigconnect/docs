# Reviewing Assets

When an asset’s workflow is activated, one or more users must review it before publication. You can assign workflow review processes to specific users or to a particular Role (for example: Portal or Site Content Reviewer). In the latter case, anyone assigned to that Role may approve or reject the submission. For example, with the out-of-the-box _Single Approver_ process, the workflow tasks are assigned to a Portal or Site Content Reviewer or anyone with administrator rights.

TIP

Bulk management of workflow items is possible using the Workflow Metrics application.

Once an asset has been submitted, the _Workflow_ application sends a notification to all the potential reviewers. To access notifications, click your avatar and select _Notifications_.

![Workflow sends a notification an asset is ready for review.](https://resources.learn.liferay.com/images/dxp/latest/en/process-automation/workflow/using-workflows/reviewing-assets/images/01.png)

### Reviewing an Asset <a href="#reviewing-an-asset" id="reviewing-an-asset"></a>

In order to begin reviewing an asset, you must assign the task to yourself. Note that when using the Single Approver workflow, tasks are by default assigned to a Role. (You can also build custom workflows which assign review tasks to specific users instead of by Roles.)

#### Assigning the Review Task <a href="#assigning-the-review-task" id="assigning-the-review-task"></a>

1. Click your avatar and select _My Workflow Tasks_.
2.  All workflow tasks assigned directly to a user are listed in the My Workflow Task widget’s _Assigned to Me_ tab.

    ![The assets assigned to a user are listed in Assigned to Me.](https://resources.learn.liferay.com/images/dxp/latest/en/process-automation/workflow/using-workflows/reviewing-assets/images/02.png)
3.  To claim a task, click the _Assigned to My Roles_ tab.

    ![The Assets assigned to Roles are listed in each associated user's Assigned to My Roles tab.](https://resources.learn.liferay.com/images/dxp/latest/en/process-automation/workflow/using-workflows/reviewing-assets/images/03.png)
4.  Click the asset’s _Actions_ button (![Actions](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) and select _Assign to Me_.

    ![Assign the task to self.](https://resources.learn.liferay.com/images/dxp/latest/en/process-automation/workflow/using-workflows/reviewing-assets/images/04.png)
5. Enter an optional comment in the _Comment_ text field.
6. Click _Done_.

#### Approving or Rejecting a Task <a href="#approving-or-rejecting-a-task" id="approving-or-rejecting-a-task"></a>

Once you have assigned to task to yourself, you can approve or reject the submission.

1. Click the name of the asset to review the asset. Here, there is a preview of the asset (a _Wiki Page_) and the review status.

<figure><img src="../../../../../.gitbook/assets/image (1073).png" alt=""><figcaption></figcaption></figure>

![Review the asset.](https://resources.learn.liferay.com/images/dxp/latest/en/process-automation/workflow/using-workflows/reviewing-assets/images/05.png)

1.  Click the _Actions_ button (![Actions](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) and select _Approve_ or _Reject_.

    ![Approve or reject the asset.](https://resources.learn.liferay.com/images/dxp/latest/en/process-automation/workflow/using-workflows/reviewing-assets/images/06.png)
2. Enter an optional comment and click _Done_.

Once a task is complete, one of several things happens:

* If the submission is rejected, then the asset is not published. The asset exits the workflow process. On the User’s _Notifications_, the original author is notified that the submission was rejected and needs to be modified before resubmitted. It is marked as _Update_ in _My Workflow Tasks_.
* If there are more than one reviewers, the task is advanced to the next reviewer in the chain.
* If the submission is approved and there is only one reviewer, then the task is moved to the _Completed_ section on the _Assigned to Me_ tab. The asset is published.

![The completed task is in Assigned to Me tab.](https://resources.learn.liferay.com/images/dxp/latest/en/process-automation/workflow/using-workflows/reviewing-assets/images/07.png)

The approved asset (the Wiki Page) is now published in the widget.

\
