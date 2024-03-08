# Enabling and Configuring Sharing

The Sharing configuration allows users to [share files](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/documents-and-media/publishing-and-sharing/managing-document-access/sharing-documents-with-other-users) with each other. You can enable file sharing at the Global, Virtual Instance, and Site scopes.

NOTE

Sharing is enabled globally by default.

### [Global Sharing Configuration](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/documents-and-media/publishing-and-sharing/managing-document-access/enabling-and-configuring-sharing#global-sharing-configuration) <a href="#global-sharing-configuration" id="global-sharing-configuration"></a>

1. Click the _Global Menu_ (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)) and click _Control Panel_.
2. Click _System Settings_.
3.  Under the _Content and Data_ section, click _Sharing_.

    ![The Sharing link leads to the settings for sharing.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/publishing-and-sharing/managing-document-access/enabling-and-configuring-sharing/images/02.png)
4. Under _System Scope_, select _Sharing_.
5. Configure the following options:
   * **Expired Sharing Entries Check Interval:** The interval in minutes for how often expired sharing entries are checked for deletion.
   * **Enabled:** Check this box to enable sharing globally.
6. Under _Virtual Instance Scope_, select _Sharing_.
7.  Check the _Enable_ box to enable sharing for all the Virtual Instances on your system. Leaving it unchecked means configuring sharing on each Virtual Instance.

    NOTE

    When you enable sharing at the System scope, you enable sharing for all the Virtual Instances on your system. To change this behavior and enable sharing on a per-instance basis, uncheck the Sharing option under the Virtual Instance scope section.

    ![Configure sharing globally.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/publishing-and-sharing/managing-document-access/enabling-and-configuring-sharing/images/03.png)
8. Click _Save_.

WARNING

If you require this feature to be disabled, [blacklist](https://learn.liferay.com/web/guest/w/dxp/system-administration/installing-and-managing-apps/managing-apps/blacklisting-osgi-components) the component. Do not enter zero or a negative number for the interval.

### [Virtual Instance Sharing Configuration](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/documents-and-media/publishing-and-sharing/managing-document-access/enabling-and-configuring-sharing#virtual-instance-sharing-configuration) <a href="#virtual-instance-sharing-configuration" id="virtual-instance-sharing-configuration"></a>

1. Click the _Global Menu_ (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)) and click _Control Panel_.
2. Click _Instance Settings_.
3.  Under _Content and Data_, click _Sharing_.

    NOTE

    The Sharing option is unavailable when Sharing is disabled at the System Scope. See [Global Sharing Configuration](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/documents-and-media/publishing-and-sharing/managing-document-access/enabling-and-configuring-sharing#global-sharing-configuration) to modify the System Scope settings.
4. Under _Virtual Instance Scope_, select _Sharing_.
5.  Check the _Enabled_ box to enable sharing on this Virtual Instance.

    ![You can enable or disable sharing for each instance.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/publishing-and-sharing/managing-document-access/enabling-and-configuring-sharing/images/04.png)
6. Click _Save_.

### [Site Sharing Configuration](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/documents-and-media/publishing-and-sharing/managing-document-access/enabling-and-configuring-sharing#site-sharing-configuration) <a href="#site-sharing-configuration" id="site-sharing-configuration"></a>

NOTE

The Site Sharing option is unavailable when Sharing is disabled at the System or Virtual Instance scopes. See [Global Sharing Configuration](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/documents-and-media/publishing-and-sharing/managing-document-access/enabling-and-configuring-sharing#global-sharing-configuration) and [Virtual Instance Sharing Configuration](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/documents-and-media/publishing-and-sharing/managing-document-access/enabling-and-configuring-sharing#virtual-instance-sharing-configuration) to modify these settings.

1. Access the Site Sharing setting.
   * In Liferay DXP 7.4+
     1. From the Site Menu, go to _Configuration_ → _Site Settings_.
     2. In the Content and Data section, click _Sharing_.
   * In previous Liferay DXP versions
     1. From the Site Menu, go to _Configuration_ → _Settings_.
     2. Under the Advanced area, expand the _Content Sharing_ section.
2. Toggle the _Enable Sharing_ option to enable or disable sharing files for your site.
3. Click _Save_ when finished.

\
