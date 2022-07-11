# System Settings

MWARE Portal is modular, meaning it’s composed of many applications divided into even smaller “chunks” of functionality. The applications, and sometimes even code chunks, are configurable at several scopes, as discussed in the introductory article for this section.

In System Settings, administrators make system scoped configuration changes and set system-wide default configurations for other scopes. System Settings is located in Control Panel → Configuration → System Settings.

![](<../../../.gitbook/assets/image (704) (1).png>)

### Editing System Configurations <a href="#editing-system-configurations" id="editing-system-configurations"></a>

System Settings is organized into sections (for example, Content) and categories (for example, Workflow) based on the functionality being configured. There’s also a Search bar to make finding configuration entries easier. Search for the name of a specific configuration entry, or even a specific field within an entry.

![](<../../../.gitbook/assets/image (711) (1).png>)

Changing a configuration isn’t difficult:

1. Find the configuration entry you need, either by searching or browsing the sections and categories.
2. Open the configuration form for the entry.
3. Make any changes you’d like, then click _Save_. Your configuration changes are saved and applied throughout the system.

**Important:** Content generated using templates (e.g., FreeMarker templates and Application Display Templates) is cached. Cached content might not reflect configuration changes until the cache is invalidated (cleared). The Server Administration → Resources tab provides cache clearing options.

![](<../../../.gitbook/assets/image (713) (1).png>)

If you make some regrettable configuration decisions and can’t recall exactly what you did, start over by clicking the actions button (![Actions](https://liferay-support.zendesk.com/hc/article\_attachments/360030186732/icon-actions.png)), then clicking _Reset Default Values_.

### Configuration Scope <a href="#configuration-scope" id="configuration-scope"></a>

While browsing the categories of System Settings, you’ll notice that clicking into a topic (for example, Blogs) reveals entries at different scopes. All the settings here act at the system scope. For scopes labeled other than System, these configurations act as defaults. In other words, they identify where the system-wide configuration is overridden. True system-scoped configurations (those under a category’s _System Scope_ heading) are not overridden anywhere. There are four values that you’ll see under Scope:

*   _System Scope:_ Any System Settings configuration made for system scoped entries becomes the final value for the application in a system-wide fashion. It affects the whole system and isn’t overridden anywhere else.

    <img src="https://liferay-support.zendesk.com/hc/article_attachments/360030186832/system-settings-system-scope.png" alt="Figure 4: Some System Settings entries are system scoped." data-size="original">

    Figure 4: Some System Settings entries are system scoped.
*   _Virtual Instance Scope:_ Configuration at the Virtual Instance level is overridden in Instance Settings.

    <img src="https://liferay-support.zendesk.com/hc/article_attachments/360030186812/system-settings-instance-scope.png" alt="Figure 5: Some System Settings are virtual instance scoped." data-size="original">

    Figure 5: Some System Settings are virtual instance scoped.
*   _Site Scope:_ Configuration at this scope is overridden in each site.

    <img src="https://liferay-support.zendesk.com/hc/article_attachments/360030186752/system-settings-site-scope.png" alt="Figure 6: Some System Settings are site scoped." data-size="original">

    Figure 6: Some System Settings are site scoped.
*   _Widget Scope:_ Configuration at this scope is overridden in each Widget Instance (like the Blogs example below).

    <img src="https://liferay-support.zendesk.com/hc/article_attachments/360030156071/system-settings-application-scope.png" alt="Figure 7: Some System Settings entries are widget scoped." data-size="original">

    Figure 7: Some System Settings entries are widget scoped.

If a configuration changed in System Settings is also configurable at a different scope, the System Settings value acts as a default that can be overridden. Once a configuration change is made at a more granular scope, making a change at the system level doesn’t do anything.

For example, allowing comments is configurable for each Blogs Entry. Set the default behavior at Control Panel → Configuration → System Settings → Blogs. In the Blogs Entry under Widget Scope, disable the _Enable Comments_ checkbox.

Now add a Blog Entry to a Site’s Content & Data → Blogs application. Then go to a public page and add the Blogs Widget to the page. Click the Options button (![Options](https://liferay-support.zendesk.com/hc/article\_attachments/360030186852/icon-app-options.png)) for the widget and select _Configuration_. You’ll see the same Enable Comments checkbox, and its default is now false (unchecked). Checking the box in the Widget Configuration screen breaks its link with the System Settings entry. Changing the System Settings configuration has no effect on this widget anymore.

### Exporting and Importing Configurations

What if you change many default configurations in System Settings, and then need to make the same changes in another installation? Don’t worry, you don’t need to remember and duplicate every choice you made. The System Settings application lets you export a single entry’s configurations, or all the settings you made in the System Settings interface. The exported files are deployable to any other installation of the same version.

To export a single entry’s configurations, click the actions button (![Actions](https://liferay-support.zendesk.com/hc/article\_attachments/360030186732/icon-actions.png)), then click _Export_. A `.config` file containing your configuration downloads to your system.

To export all the configuration changes you’ve made in System Settings, click the System Settings options button (![Options](https://liferay-support.zendesk.com/hc/article\_attachments/360030186792/icon-options.png)), then click _Export All Settings_. The `.config` files for all the entries you edited then download in a ZIP file.

To make these configurations active in the destination system, unzip and place the `.config` files in the `[Portal_Home]/osgi/configs` folder.

Now you know what System Settings is and how to use it. All that’s left is to explore the entries to see what configuration options are available. If you aren’t sure what something does, check the documentation for the feature you’re interested in, as specific configurations are covered there.
