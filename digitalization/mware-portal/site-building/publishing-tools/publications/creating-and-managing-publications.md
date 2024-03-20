# Creating and Managing Publications

With Publications, users can create instance-scoped blocks of changes. Because each publication is instance-scoped, they can be accessed anywhere in the portal UI and include changes from multiple Sites. By default, users can only view and manage publications they’ve created. However, users can invite each other to collaborate on their publications. See Collaborating on Publications for more information.

Once Publications is enabled, you can create, edit, publish, or remove a publication via the Publications application page or the Publications drop-down menu.

IMPORTANT

Before upgrading your version, ensure you’ve published all active publications. After upgrading, active publications are marked as out of date and cannot be published or changed. Also, you cannot revert any publications published before your upgrade. The _Revert_ option is removed for these publications.

### Creating a Publication <a href="#creating-a-publication" id="creating-a-publication"></a>

1.  Click the _Publications_ drop-down menu, and select _Create New Publication_.

    ![Click Create New Publication in the Publications drop-down menu.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/publishing-tools/publications/creating-and-managing-publications/images/01.png)

    Alternatively, open the Publications application and click _Add_ (![Add button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png)) in the _Ongoing_ tab.
2.  Enter a _name_ and _description_ for the publication.

    Alternatively, select a publication template to auto-populate these fields.

    NOTE

    Publication names do not need to be unique. However, using unique names is recommended for organization.
3. Click _Create_.

![Enter a name and description for your publication, and click Create.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/publishing-tools/publications/creating-and-managing-publications/images/02.png)

Once the publication is created, you can begin adding changes to it. See Making and Publishing Changes to learn more about the editing and publishing process.

NOTE

After creation, each publication is automatically updated with changes made in production, provided they don’t conflict with any changes made in the publication.

### Managing Ongoing Publications <a href="#managing-ongoing-publications" id="managing-ongoing-publications"></a>

In the _Ongoing_ tab, you can view a table of all active, unpublished publications that you have permission to view. The table is ordered by _Modified Date_ and includes each publication’s name, description, time of last modification, time of creation, and owner.

![The Ongoing tab lists all active, unpublished publications.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/publishing-tools/publications/creating-and-managing-publications/images/03.png)

From here, you can click a publication’s Actions button ( ![Actions button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png) ) to perform the following actions:

**Work on Publication**: Select the publication to add changes to it. Once selected, you can easily toggle between the publication and production via the Publications drop-down menu. See Making Changes in a Publication for more information.

**Edit**: Modify the publication’s name or description.

**Review Changes**: View all changes included in the publication. See Reviewing Publication Changes for more information.

**Invite Users**: Invite users to collaborate on the publication. See Collaborating on Publications for more information.

**Publish**: Begin the publishing process for the publication. This redirects users to the publishing page, where they can resolve conflicts. If there are no conflicts, they can click _Publish_ to make their changes live. See Publishing Changes and Resolving Conflicts for more information.

**Schedule**: Schedule the publication’s changes to be applied to production. See Publishing Changes for more information.

**Permissions**: Assign publication-specific permissions to User roles. Permissions configured in this way are scoped to the individual publication.

**Delete**: Remove the publication from your instance. When selected, you are prompted to confirm your choice.

WARNING

Deleting a publication permanently removes all of its changes from your database and cannot be undone. Before deleting a publication, ensure you’ve saved any data you want to preserve.

### Managing Scheduled Publications <a href="#managing-scheduled-publications" id="managing-scheduled-publications"></a>

In the _Schedule_ tab, you can view a table of all scheduled publications that you have permission to view. By default, scheduled publications are listed by _Name_, though you can order them by modified date or when each publication is scheduled for Publishing.

![The Schedule tab lists all scheduled publications for your instance.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/publishing-tools/publications/creating-and-managing-publications/images/04.png)

Once scheduled, changes cannot be added to a publication. However, you can click its Actions button (![Actions button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) to perform the following actions:

**Unschedule**: Cancel the scheduled publishing and return the publication to the _In Progress_ status.

**Reschedule**: Set an alternative date and time for publishing the changes to production.

**Review Changes**: View all changes included in the publication.

### Managing History of Publications <a href="#managing-history-of-publications" id="managing-history-of-publications"></a>

In the _History_ tab, you can view a table of all published publications for the portal instance, including those published by other users. By default, publications are ordered by _Published Date_, though you can order them by name.

![The History tab lists all previously published publications for your instance.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/publishing-tools/publications/creating-and-managing-publications/images/05.png)

From here, you can click _Revert_ for a publication to automatically create a new publication that reverts its changes. You can also make additional changes to your portal instance as part of your Revert publication. See Reverting Changes for more information.

NOTE

While all published publications on an instance are listed in the History tab, only publication owners and collaborators can view the publication’s list of changes or revert it.

\
