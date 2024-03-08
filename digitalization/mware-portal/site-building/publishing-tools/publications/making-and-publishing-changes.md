# Making and Publishing Changes

Publications provides a convenient way to manage changes to your Liferay instance. With it, you and your team can create and collaborate on blocks of changes called ‘publications’. You can then publish those changes to production when ready.

By default, users can only contribute to publications they’ve created. However, they can invite other users to collaborate on their publications. See [Collaborating on Publications](https://learn.liferay.com/web/guest/w/dxp/site-building/publishing-tools/publications/collaborating-on-publications) for more information.

If you haven’t already created a publication to track changes, see [Creating a Publication](https://learn.liferay.com/web/guest/w/dxp/site-building/publishing-tools/publications/creating-and-managing-publications#creating-a-publication) for more information.

Once created, a publication is automatically updated with changes made to production, provided they don’t conflict with changes made in the publication. This ensures you and your team are working on the latest version of production.

IMPORTANT

After upgrading your Liferay version, existing publications cannot be changed, published, or reverted. All active publications are marked as out of date and can only be viewed or deleted. Also, the _Revert_ option is removed for publications published before your upgrade.

### [Making Changes in a Publication](https://learn.liferay.com/web/guest/w/dxp/site-building/publishing-tools/publications/making-and-publishing-changes#making-changes-in-a-publication) <a href="#making-changes-in-a-publication" id="making-changes-in-a-publication"></a>

Before making changes to your instance, ensure you’re working on the correct publication. You can confirm this via the Publications bar. If you’re not currently in a publication, you can create one or select an existing one.

![Select the desired publication or create one.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/publishing-tools/publications/making-and-publishing-changes/images/01.png)

When you add, edit, or remove assets in a publication, your changes are only visible in the publication where they’re made. When you delete an asset in a publication, DXP marks it for deletion and hides it when viewing the publication. The asset remains viewable to production and other publications, since its removal is only a logical deletion.

While working on a publication, changes made to supported Liferay components are tracked in the publication. This includes changes across multiple sites.

IMPORTANT

When editing different sites in the same publication, keep in mind that all tracked changes are applied to production when the publication is published.

#### [Navigating to Unsupported Applications](https://learn.liferay.com/web/guest/w/dxp/site-building/publishing-tools/publications/making-and-publishing-changes#navigating-to-unsupported-applications) <a href="#navigating-to-unsupported-applications" id="navigating-to-unsupported-applications"></a>

[Release Feature](https://learn.liferay.com/web/guest/w/dxp/system-administration/configuring-liferay/feature-flags#release-feature-flags)

Liferay 7.4 2023.Q3+/GA92+

If you navigate to an application that does not support Publications, Liferay indicates that changes in the application cannot be saved to a publication.

For some applications, you can directly edit production while in a publication.

![Directly edit production while in a publication.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/publishing-tools/publications/making-and-publishing-changes/images/02.png)

For other applications, you must switch to production before you can make changes.

![Switch to production before making changes in the application.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/publishing-tools/publications/making-and-publishing-changes/images/03.png)

### [Making Changes in Parallel Publications](https://learn.liferay.com/web/guest/w/dxp/site-building/publishing-tools/publications/making-and-publishing-changes#making-changes-in-parallel-publications) <a href="#making-changes-in-parallel-publications" id="making-changes-in-parallel-publications"></a>

Users can have multiple active publications at the same time. Changes made in these parallel publications create alternative edit histories for your instance’s entities. These changes can be compatible or conflicting, depending on whether they modify the same or related content.

#### [Compatible Changes](https://learn.liferay.com/web/guest/w/dxp/site-building/publishing-tools/publications/making-and-publishing-changes#compatible-changes) <a href="#compatible-changes" id="compatible-changes"></a>

If parallel publications include compatible changes, they can be published without issue. In this case, the tracked changes of both publications are applied to production.

#### [Conflicting Changes](https://learn.liferay.com/web/guest/w/dxp/site-building/publishing-tools/publications/making-and-publishing-changes#conflicting-changes) <a href="#conflicting-changes" id="conflicting-changes"></a>

If parallel publications include incompatible changes and one of the publications is applied to production first, then the other publication will encounter publishing conflicts. These conflicts can also be caused when production is updated directly. See [Resolving Conflicts](https://learn.liferay.com/web/guest/w/dxp/site-building/publishing-tools/publications/resolving-conflicts) for more information on this process.

### [Reviewing Publication Changes](https://learn.liferay.com/web/guest/w/dxp/site-building/publishing-tools/publications/making-and-publishing-changes#reviewing-publication-changes) <a href="#reviewing-publication-changes" id="reviewing-publication-changes"></a>

Before publishing, you can review a publication’s tracked changes to ensure it only includes the desired edits. To do this, open the Publications drop-down menu and select _Review Changes_.

![Open the Publications drop-down menu and select Review Changes.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/publishing-tools/publications/making-and-publishing-changes/images/04.png)

Alternatively, you can review any publication’s tracked changes via the Publications application. Simply navigate to the _Ongoing_ tab, click on the _Actions_ button ( ![Actions button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png) ) for the desired Publication, and select _Review Changes_.

When reviewing changes, all tracked changes appear in a list that you can order by User, Site, Title, Type, Status, Change Type, or Last Modified.

By default, Publications only displays some changes, but you can view all system changes as well. Click _Actions_ (![Action Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) for the publication and select _Show System Changes_. This displays all changes included in the publication.

![Select Show System Changes to view all changes included in the publication.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/publishing-tools/publications/making-and-publishing-changes/images/05.png)

During review, you can select a change to view more details and access additional actions (e.g., edit, discard). Available actions depend on the entity’s type.

![Select a change to view more details and access additional actions.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/publishing-tools/publications/making-and-publishing-changes/images/06.png)

#### [Previewing Changes to Pages](https://learn.liferay.com/web/guest/w/dxp/site-building/publishing-tools/publications/making-and-publishing-changes#previewing-changes-to-pages) <a href="#previewing-changes-to-pages" id="previewing-changes-to-pages"></a>

[Release Feature](https://learn.liferay.com/web/guest/w/dxp/system-administration/configuring-liferay/feature-flags#release-feature-flags)

Liferay 7.4 2023.Q3+/GA92+

If a publication includes edits to a page, you can preview those changes alongside the original version of the page.

![View page versions in the Display tab.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/publishing-tools/publications/making-and-publishing-changes/images/07.png)

When reviewing changes to a page with alternate [user experiences](https://learn.liferay.com/web/guest/w/dxp/site-building/personalizing-site-experience/experience-personalization/creating-and-managing-experiences), you can use the Experiences drop-down menu to switch between them.

![Select the desired user experience to review.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/publishing-tools/publications/making-and-publishing-changes/images/08.png)

Once selected, you can view each page version individually or select Split View to review them side by side.

![Review page versions individually or select Split View to review them side by side.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/publishing-tools/publications/making-and-publishing-changes/images/09.png)

#### [Moving Changes Between Publications](https://learn.liferay.com/web/guest/w/dxp/site-building/publishing-tools/publications/making-and-publishing-changes#moving-changes-between-publications) <a href="#moving-changes-between-publications" id="moving-changes-between-publications"></a>

[Beta Feature](https://learn.liferay.com/web/guest/w/dxp/system-administration/configuring-liferay/feature-flags#beta-feature-flags)

Liferay 7.4 2023.Q3+/GA92+

While reviewing changes, you can move them to different publications. This provides greater flexibility when managing content:

1.  Click the _Actions_ button ( ![Actions Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png) ) for the desired change and select _Move Changes_.

    ![Click the Actions button for the desired change and select Move changes.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/publishing-tools/publications/making-and-publishing-changes/images/10.png)
2. Select the desired _publication_.
3. Click _Move_.

This moves the change into the selected publication along with its children. It also removes the change and its children from the current publication.

### [Publishing Your Changes](https://learn.liferay.com/web/guest/w/dxp/site-building/publishing-tools/publications/making-and-publishing-changes#publishing-your-changes) <a href="#publishing-your-changes" id="publishing-your-changes"></a>

When you’ve finished making your changes, you can publish them immediately or schedule them to be published at a later time. Once published, a publication is listed in the _History_ tab of the Publications application. This maintains a clear audit trail that you can use review and revert site changes when needed. See [Reverting Changes](https://learn.liferay.com/web/guest/w/dxp/site-building/publishing-tools/publications/reverting-changes) for more information.

When you begin the publishing process, Liferay performs a compatibility check to determine if there are any conflicts between the publication and production. If there are conflicts, DXP notifies you. While some conflicts are resolved automatically, others require manual resolution. Resolving all conflicts is necessary for publishing changes. See [Resolving Conflicts](https://learn.liferay.com/web/guest/w/dxp/site-building/publishing-tools/publications/resolving-conflicts) for more information.

#### [Publishing Now](https://learn.liferay.com/web/guest/w/dxp/site-building/publishing-tools/publications/making-and-publishing-changes#publishing-now) <a href="#publishing-now" id="publishing-now"></a>

Follow these steps to publish changes immediately:

1.  Open the Publications drop-down menu, and select _Review Changes_.

    Alternatively, open the Publications application, and click on the desired publication.
2.  Click _Publish_.

    ![Navigate to the desired publication and click on Publish.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/publishing-tools/publications/making-and-publishing-changes/images/11.png)
3.  If necessary, manually resolve any detected conflicts. Once all conflicts are resolved, you can proceed.

    ![Resolve all conflicts before publishing.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/publishing-tools/publications/making-and-publishing-changes/images/12.png)
4. Click _Publish_.

The publication’s changes are immediately applied to production, and you are redirected to the History tab. From here, you can view publishing details (e.g., date, publisher, status) and [revert](https://learn.liferay.com/web/guest/w/dxp/site-building/publishing-tools/publications/reverting-changes) a publication’s changes if desired.

#### [Scheduling for Later](https://learn.liferay.com/web/guest/w/dxp/site-building/publishing-tools/publications/making-and-publishing-changes#scheduling-for-later) <a href="#scheduling-for-later" id="scheduling-for-later"></a>

Follow these steps to schedule publishing for a later time.

1.  Open the Publications drop-down menu, and select _Review Changes_.

    Alternatively, open the Publications application, and click on the desired publication.
2.  Click _Schedule_.

    ![Navigate to the desired publication and click on Schedule.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/publishing-tools/publications/making-and-publishing-changes/images/13.png)
3.  If necessary, manually resolve any detected conflicts. Once all conflicts are resolved, you can proceed.

    IMPORTANT

    At the time of the publication, DXP performs a second conflict detection to ensure there are no new conflicting changes. The publish fails if DXP detects new conflicts that cannot be automatically resolved. If the conflicts can be resolved automatically, DXP proceeds with publishing.
4.  Enter a date and time to determine when the changes are published to production.

    ![Set a date and time to publish your changes.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/publishing-tools/publications/making-and-publishing-changes/images/14.png)
5. Click _Schedule_ to complete the process.

This redirects you to the Scheduled tab where you can view and [manage scheduled publications](https://learn.liferay.com/web/guest/w/dxp/site-building/publishing-tools/publications/creating-and-managing-publications#managing-scheduled-publications).

### [Removing Unnecessary Publication Data](https://learn.liferay.com/web/guest/w/dxp/site-building/publishing-tools/publications/making-and-publishing-changes#removing-unnecessary-publication-data) <a href="#removing-unnecessary-publication-data" id="removing-unnecessary-publication-data"></a>

When reviewing publication changes, file and page previews can accumulate in the document library. If desired, you can remove this data.

1. Open the _Global Menu_ (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)), go to _Control Panel_, and click _System Settings_.
2. Click _Upgrades_ under Platform.
3. Go to the _Data Removal_ tab.
4.  Select _Remove DL Preview Change Tracking Store Content Data_.

    ![Check the change tracking options.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/publishing-tools/publications/making-and-publishing-changes/images/15.png)
5. Click _Save_.

Liferay immediately removes the unnecessary data.

\
