# Using Workflow with Accounts

Accounts are fully integrated with Liferay’s [workflow engine](https://learn.liferay.com/web/guest/w/dxp/process-automation/workflow/introduction-to-workflow) so you can enable an approval process for account creation.

To enable a workflow for Accounts,

1. Open the _Global Menu_ (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)), go to the _Applications_ tab → _Process Builder_.
2. Go to the _Configurations_ tab and click _Edit_ for Account Entry.
3.  Use the drop-down menu to select a _workflow_.

    See [Workflow Designer Overview](https://learn.liferay.com/web/guest/w/dxp/process-automation/workflow/designing-and-managing-workflows/workflow-designer/workflow-designer-overview) to learn about designing custom workflows.

    ![Use the drop-down menu to select a workflow.](https://resources.learn.liferay.com/images/dxp/latest/en/users-and-permissions/accounts/using-workflow-with-accounts/images/01.png)
4. Click _Save_.

The workflow is now active. New accounts must go through the defined approval process before they are activated.

When [creating an account](https://learn.liferay.com/web/guest/w/dxp/users-and-permissions/accounts#creating-an-account), its status is set to Pending and reviewers are notified.

![Designated reviewers are notified of new accounts.](https://resources.learn.liferay.com/images/dxp/latest/en/users-and-permissions/accounts/using-workflow-with-accounts/images/02.png)

Reviewers can approve or reject the pending account. See [Reviewing Assets](https://learn.liferay.com/web/guest/w/dxp/process-automation/workflow/using-workflows/reviewing-assets) for more information on the review process.

![Reviewers can approve or reject accounts.](https://resources.learn.liferay.com/images/dxp/latest/en/users-and-permissions/accounts/using-workflow-with-accounts/images/03.png)
