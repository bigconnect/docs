# Workflow

MWARE Portal ships with a workflow engine that allows users to review and approve content such as Blogs, Web Content, Wikis before they are published. The Workflow application makes it possible to define any number of business processes/workflows, deploy them, and manage them through a portal interface. Workflow processes are written in `XML` format and several sample workflow processes are available for reference. Lastly, workflow processes can integrate with Users, Groups, and Roles as reviewers and approvers. After you defined your review process, you can select which types of content require a review process before publication.

### Activating Workflow <a href="#activating-workflow" id="activating-workflow"></a>

When a workflow is activated for a particular asset, the _Publish_ button is replaced by a _Submit for Workflow_ button, and instead of instant publication, the asset’s status is set as _Pending_. It must proceed through the workflow before publication.

![Instead of a Publish button, a Submit for Workflow button appears for workflow-enabled resources.](https://resources.learn.liferay.com/images/dxp/latest/en/process-automation/workflow/introduction-to-workflow/images/01.png)

The out-of-the-box workflow process is called _Single Approver_; that means one person is required to review and approve submissions before publication.

### Understanding Workflow Status <a href="#understanding-workflow-status" id="understanding-workflow-status"></a>

An asset in a workflow process always has a status. The status is used to determine important things like whether the asset can be displayed to end users. There are many statuses, but some of the most common and important ones to understand are described here:

* The _draft_ status is assigned to content that can be saved before being ready for either review in the workflow or for viewing by end users.
* The _pending_ status denotes that the asset is in a workflow process that’s not yet completed.
* The _approved_ status means the asset is ready for display in the UI for all user’s with permission to see it.
* The _denied_ status is for assets that have failed to make it through a workflow. This status may be used to trigger a notification to the original submitter of the asset, perhaps to prompt a revision and resubmission of the content.

![Content can have a workflow status.](https://resources.learn.liferay.com/images/dxp/latest/en/process-automation/workflow/introduction-to-workflow/images/03.png)

#### Versioned Assets Can Have Multiple Statuses <a href="#versioned-assets-can-have-multiple-statuses" id="versioned-assets-can-have-multiple-statuses"></a>

Versioned assets can have two statuses.

![Versioned assets can have two statuses.](https://resources.learn.liferay.com/images/dxp/latest/en/process-automation/workflow/introduction-to-workflow/images/04.png)

Most asset’s default to displaying the newest approved version. If an already approved asset is updated, a new version is created and sent through the workflow. While the new version is not yet approved in the workflow, a second status is displayed for the asset (e.g., _pending_). The last approved version will continue to be used until the new version is approved.

\




