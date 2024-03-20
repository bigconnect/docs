# Propagating Template Changes

When site template propagation is enabled, you can make changes to the template’s pages and automatically propagate those changes to connected sites. However, making changes to a site page inherited from a template prevents the propagation of template changes to that page. To restore propagation for the page, you must discard changes made to it in the site. If you’ve edited multiple inherited pages and you’d like to restore propagation from the site template, you must reset changes for each one individually.

NOTE

By default, propagation is triggered for each connected site individually, when it is first visited by a user after changes are made. However, you can configure your system to also trigger propagation to connected sites simultaneously whenever users toggle a template to _Ready for Propagation_. See Configuring Site Template Propagation for more information.

### Which Template Changes are Propagated? <a href="#which-template-changes-are-propagated" id="which-template-changes-are-propagated"></a>

Only changes to a template’s pages and page elements are propagated to connected sites. This includes newly created pages as well as any changes to fragments and widgets in existing pages. Other application data is only copied at initial site creation; subsequent changes are not automatically propagated.

For this reason, site templates are primarily intended for maintaining pages across multiple sites. If you must create and maintain web content and other assets across sites, use asset libraries.

### Enabling and Disabling Propagation <a href="#enabling-and-disabling-propagation" id="enabling-and-disabling-propagation"></a>

When creating a template, you determine whether to enable or disable automatic propagation, but you can change this setting at any time.

![Enable or disable automatic propagation during template creation.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/sites/site-templates/propagating-template-changes/images/01.png)

Follow these steps to enable or disable propagation after template creation:

1. Open the _Global Menu_ (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)), go to the _Control Panel_ tab, and click _Site Templates_.
2. Select the desired template.
3.  Click the _Propagation_ button (![Propagation Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-merge.png))

    If propagation is enabled, click _Disable Propagation_ to prevent template changes from being automatically propagated to connected site pages.

    ![If Propagation is enabled, click Disable Propagation.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/sites/site-templates/propagating-template-changes/images/02.png)

    If propagation is disabled, click _Ready for Propagation_ to enable automatic propagation.

    ![If Propagation is disabled, click Ready for Propagation.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/sites/site-templates/propagating-template-changes/images/03.png)

Alternatively, you can enable or disable automatic propagation via the site templates application page. Just click _Actions_ (![Actions Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) for the desired template and select _Disable Propagation_ or _Ready for Propagation_.

![Click the Actions button for the desired site template and select Disable Propagation or Ready for Propagation.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/sites/site-templates/propagating-template-changes/images/04.png)

### Reconnecting Inherited Pages to Their Template <a href="#reconnecting-inherited-pages-to-their-template" id="reconnecting-inherited-pages-to-their-template"></a>

If you make changes to a site page inherited from a template, it is disconnected from the template’s page and is not automatically propagated. To reconnect the site page to the template, you must discard your changes to the site page.

Follow these steps to discard changes to a site page and resync it with its template:

1. Navigate to the site page.
2.  Click the _Aditional Information_ icon (![Information](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-control-menu-information.png)) in the Application bar.

    ![Click the Aditional Information icon and select Reset Changes.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/sites/site-templates/propagating-template-changes/images/05.png)
3.  Click _Reset Changes_.

    WARNING

    Resetting changes **reverts any changes made to the site page directly**. This can result in data loss. Proceed with caution.

\
