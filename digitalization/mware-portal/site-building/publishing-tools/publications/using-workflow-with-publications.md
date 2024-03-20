# Using Workflow with Publications

Publications is integrated with Workflow framework, so you can continue to use custom workflows for reviewing content and pages while Publications is enabled. This ensures all tracked changes go through the desired approval process before publishing them to production.

![Publications is fully integrated with Workflow framework.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/publishing-tools/publications/using-workflow-with-publications/images/01.png)

With Publications, changes are scoped to the environment where they are made (i.e., production or a publication). Content reviewers must have access to the appropriate environment to review and approve changes. For example, if site contributors make changes in a publication, the workflow reviewers must also have access to that publication. See Collaboration on Publications for more information about inviting users to a publication and assigning them permissions.

When contributors submit changes for approval, the workflow notification includes the environment where the changes were made. Reviewers must then switch to the appropriate environment before viewing and approving the changes.

![Workflow notifications indicate the environment where changes were made.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/publishing-tools/publications/using-workflow-with-publications/images/02.png)

Reviewers must approve all changes before publishing them to production. Attempting to publish unapproved changes displays an error message.

![Attempting to publish unapproved changes displays an error message.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/publishing-tools/publications/using-workflow-with-publications/images/03.png)

Once changes are approved, additional edits to the same entities are tracked as separate changes in the publication, each with its own workflow status.

![Additional edits to the same entities are tracked as separate changes in the publication.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/publishing-tools/publications/using-workflow-with-publications/images/04.png)
