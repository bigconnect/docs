# Propagation of Changes

If you make an update to a Page Fragment or Content Page Template, it doesn’t automatically propagate changes, but you can access the _Usages and Propagation_ page to selectively propagate changes.

1. From the Site Administration menu, go to _Site Builder_ → _Page Fragments_
2. Select the Collection containing the changed fragment.
3. Open the _Actions_ menu (![Actions](https://liferay-support.zendesk.com/hc/article\_attachments/360040158732/icon-actions.png)) for the Fragment and select _View Usages_.

The _Usages and Propagation_ page shows a list of every Page, Page Template, and Display Page that uses the selected Page Fragment. You can then selectively propagate Fragment changes to any or all of the pages listed. You can use the various filters and selection options to apply updates to pages quickly.

![Figure 1: Viewing the Usages and Propagation page.](https://liferay-support.zendesk.com/hc/article\_attachments/360030157671/fragment-usages-and-propagation.png)

To update a page or template,

1. Select the page or pages you want to update by checking the box next to the page name.
2.  Click the _Propagate_ icon ( ![Propagate](https://liferay-support.zendesk.com/hc/article\_attachments/360040164011/icon-propagate.png))

    After you propagate changes, visit any affected page to verify there were no unexpected side effects of the changes.

    **Note:** Developers or others testing a Fragment can enable Fragment change propagation from the Control Panel. It’s recommended to only leverage this functionality during testing, as automatic propagation on the production environment can cause unintended consequences.

    Changes to existing `editable` fields are not propagated since this overwrites content currently in content pages. To force propagation to content in an `editable` field, a developer must change the field ID. Any content created in that field no longer appears in the Content Page when the changes are propagated, but it remains in the database and can be retrieved using the old ID.
