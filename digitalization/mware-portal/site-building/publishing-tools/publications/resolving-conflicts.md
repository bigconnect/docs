# Resolving Conflicts

When you begin the publishing process, Liferay performs a compatibility check to determine if there are any conflicts between your publication and production. While some conflicts can be resolved automatically, others require manual resolution. Resolving all conflicts is necessary for publishing changes.

{% hint style="info" %}
Publishing conflicts only occur when changes are made to production _after_ changes have already been made in a publication. This is because each publication is automatically updated with changes made to production, provided those changes don’t conflict with changes made in the publication.
{% endhint %}

### [Automatically Resolving Conflicts](https://learn.liferay.com/web/guest/w/dxp/site-building/publishing-tools/publications/resolving-conflicts#automatically-resolving-conflicts) <a href="#automatically-resolving-conflicts" id="automatically-resolving-conflicts"></a>

For some conflicts, Liferay can provide an automatic resolution. Essentially, an automatic resolution is a pre-configured solution to certain kinds of conflicts, so you can publish your changes without further action. These solutions may include adding, modifying, or removing content from the current publication, or replacing the version of content in production with your publication’s version.

When an automatic solution is available, you’ll be notified during publishing. You can then click on _View_ to review the solution’s details. To implement an automatic solution, simply click on _Publish_.

![To implement an automatic solution, simply click on Publish.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/publishing-tools/publications/resolving-conflicts/images/01.png)

### [Manually Resolving Conflicts](https://learn.liferay.com/web/guest/w/dxp/site-building/publishing-tools/publications/resolving-conflicts#manually-resolving-conflicts) <a href="#manually-resolving-conflicts" id="manually-resolving-conflicts"></a>

For some conflicts, Liferay cannot provide an automatic resolution. This means you must perform an additional action to resolve the conflict before you can finish publishing. When a manual resolution is necessary, Publications identifies the conflict type and prompts you to resolve it.

![Edit or discard the publication's conflicting changes.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/publishing-tools/publications/resolving-conflicts/images/02.png)

Depending on the [conflict type](https://learn.liferay.com/web/guest/w/dxp/site-building/publishing-tools/publications/resolving-conflicts#understanding-conflict-types), Publications may prompt you to perform one of the following actions.

* **Edit**: This redirects you to the cause of the conflict in the current publication. From there, you can make the changes necessary to resolve the conflict.
* **Discard**: This displays a list of all changes that will be discarded. You can then click on _Discard_ to permanently remove them from your publication.

WARNING

Discarding is permanent and cannot be reversed. Ensure you’ve saved any changes you want to keep before discarding them from your publication.

### [Understanding Conflict Types](https://learn.liferay.com/web/guest/w/dxp/site-building/publishing-tools/publications/resolving-conflicts#understanding-conflict-types) <a href="#understanding-conflict-types" id="understanding-conflict-types"></a>

Users can encounter six general types of conflicts: _Duplication_, _Modification_, _Deletion Modification_, _Modification Deletion_, _Missing Requirement_, and _Addition_.

#### [Duplication Conflicts](https://learn.liferay.com/web/guest/w/dxp/site-building/publishing-tools/publications/resolving-conflicts#duplication-conflicts) <a href="#duplication-conflicts" id="duplication-conflicts"></a>

![Publications notifies you to resolve Duplication conflicts.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/publishing-tools/publications/resolving-conflicts/images/03.png)

These conflicts occur when something in production uses the same unique value of a change made in the publication. This can be caused by any type of component that requires a unique value (e.g., a folder, url, role name).

Consider the following example. Suppose you’ve added a new folder named ‘Marketing’ to a publication, while a user in production creates a folder using the same name. Since folders in the same repo must have a unique name, attempting to publish your change will result in a duplication conflict.

To resolve duplication conflicts, users can either edit the value in their publication, or discard their change.

#### [Modification Conflicts](https://learn.liferay.com/web/guest/w/dxp/site-building/publishing-tools/publications/resolving-conflicts#modification-conflicts) <a href="#modification-conflicts" id="modification-conflicts"></a>

![Publications notifies you to resolve Modification conflicts.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/publishing-tools/publications/resolving-conflicts/images/04.png)

These conflicts occur when an entity you’ve modified in a publication is then modified in production (e.g., when an edited Web Content article is moved). While some modification conflicts can be automatically resolved, others require manual resolution.

To resolve modification conflicts, you can edit the conflicting material, discard the conflicting changes in your publication, or replace the version in production.

#### [Deletion Modification and Modification Deletion Conflicts](https://learn.liferay.com/web/guest/w/dxp/site-building/publishing-tools/publications/resolving-conflicts#deletion-modification-and-modification-deletion-conflicts) <a href="#deletion-modification-and-modification-deletion-conflicts" id="deletion-modification-and-modification-deletion-conflicts"></a>

![Publications notifies you to resolve Deletion Modification and Modification Deletion conflicts.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/publishing-tools/publications/resolving-conflicts/images/05.png)

These conflicts occur when an entity is modified in a publication and then deleted in production (Deletion Modification), or vice versa (Modification Deletion). The only provided resolution for this conflict is to discard the publication’s conflicting change.

#### [Missing Requirement Conflicts](https://learn.liferay.com/web/guest/w/dxp/site-building/publishing-tools/publications/resolving-conflicts#missing-requirement-conflicts) <a href="#missing-requirement-conflicts" id="missing-requirement-conflicts"></a>

![Publications notifies you to resolve Missing Requirement conflicts.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/publishing-tools/publications/resolving-conflicts/images/06.png)

These conflicts occur when a child entity is created or edited in your publication, while its parent has been removed from production. Examples include deleting a folder from production when your publication adds or modifies a sub-folder.

To resolve Missing Requirement conflicts, you can discard the conflicting changes from your publication or revert the deletion in production. See [Reverting Changes](https://learn.liferay.com/web/guest/w/dxp/site-building/publishing-tools/publications/reverting-changes) for how to revert changes published to production.

#### [Addition Conflict](https://learn.liferay.com/web/guest/w/dxp/site-building/publishing-tools/publications/resolving-conflicts#addition-conflict) <a href="#addition-conflict" id="addition-conflict"></a>

![Publications notifies you to resolve Addition conflicts.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/publishing-tools/publications/resolving-conflicts/images/07.png)

These conflicts only occur when users attempt to revert a publication including a deletion that has already been reverted. Users cannot revert the same publication twice if it included a deletion, since the system cannot add back what has already been added back.

\
