# System Settings

You can make System-scoped configurations from Control Panel → Configuration → System Settings.

The System Settings application shows configuration sections underneath four categories: Platform, Security, Content and Data, and Other. Each section contains one or more configuration entries.

Navigate to the configuration you need by finding a section (e.g., Content and Data) of interest, then browse its categories (e.g., Workflow).

![System Settings entries are organized by section (e.g., Content and Data) and category (e.g., Assets).](https://resources.learn.liferay.com/images/dxp/latest/en/system-administration/configuring-liferay/system-settings/images/02.png)

If you can’t locate a System Settings entry by browsing, search for it using the search bar.

![You can search for System Settings entries.](https://resources.learn.liferay.com/images/dxp/latest/en/system-administration/configuring-liferay/system-settings/images/03.png)

NOTE

Some system-scoped configurations can be made in properties files and in the Server Administration application in the Control Panel.

The portal contains many applications. The applications and their services may be configurable at several [scopes](https://learn.liferay.com/web/guest/w/dxp/system-administration/configuring-liferay/understanding-configuration-scope), as defined by the application’s developers. Configuration made in System Settings is system scoped or sets system-wide default configurations that can be overridden at another scope.

If you don’t know what a configuration option does, check the documentation for that feature.

### Editing System Configurations <a href="#editing-system-configurations" id="editing-system-configurations"></a>

Once you find the configuration to modify,

1. Open the configuration form for the entry.
2.  Make any changes, then click _Save_. Your configuration changes are saved and applied throughout the system.

    IMPORTANT

    Content generated using templates (e.g., FreeMarker templates and Application Display Templates) is cached. Cached content might not reflect configuration changes until the cache is invalidated (cleared). The Server Administration -> Resources tab provides cache clearing options.

### Resetting a System Settings Entry <a href="#resetting-a-system-settings-entry" id="resetting-a-system-settings-entry"></a>

System settings can be reset to their default value by clicking the actions button (![Actions](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)), then clicking _Reset Default Values_.

![After saving changes to a configuration, the actions Reset Default Values and Export are made available.](https://resources.learn.liferay.com/images/dxp/latest/en/system-administration/configuring-liferay/system-settings/images/04.png)

### Exporting and Deploying Configurations <a href="#exporting-and-deploying-configurations" id="exporting-and-deploying-configurations"></a>

System Settings are portable. To apply the same configurations in another installation, you can export configurations; either a single configuration entry or all the entries that you changed. The exported files are deployable to any portal installation of the same version.

To export a single entry’s configurations, click the actions button (![Actions](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)), then click _Export_. A `.config` file containing your configuration downloads to your system.

To export all the configuration changes you’ve made in System Settings, click the System Settings options button (![Options](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-options.png)), then click _Export All Settings_. The `.config` files for all the entries you edited then download in a ZIP file.

To make these configurations active in the destination system, unzip and place the `.config` files in the `[Home]/osgi/configs` folder.
