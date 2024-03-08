# Configuring Site Template Propagation

By default, site template changes are propagated automatically to a connected site when a user visits the site after the template is updated. However, you can configure an additional manual trigger that propagates to all connected sites simultaneously. To enable manual propagation, use the Trigger Propagation setting. When enabled, clicking the [Ready for Propagation](https://learn.liferay.com/web/guest/w/dxp/site-building/sites/site-templates/propagating-template-changes#enabling-and-disabling-propagation) button in the site template editor triggers propagation to all connected sites. Enabling this setting does not stop the automatic propagation of template changes that occurs when users visit connected sites.

IMPORTANT

Automatic propagation can be resource intensive, depending the scope of changes made to a template.

To configure site template propagation,

1. Open the _Global Menu_ (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)), go to the _Control Panel_ tab, and click _System Settings_ under Configuration.
2. Under Platform, go to _Infrastructure_ settings and click _Site Templates_ under Virtual Instance Scope in the side menu.
3.  Check _Trigger Propagation_.

    If unchecked, changes are propagated to sites individually when they’re first visited by a user.

    If checked, enabling automatic propagation in a template also triggers propagation for all connected sites simultaneously.

    ![Check Trigger Propagation.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/sites/site-templates/configuring-site-template-propagation/images/01.png)
4. Click _Save_.

If propagation is disabled for a site template, enabling propagation immediately begins propagating changes to connected sites simultaneously. While propagation remains enabled for the template, subsequent changes are propagated to sites individually when they’re first visited by a user.
