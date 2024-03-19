# Configuring the Portal

Settings for Liferay DXP’s services and applications are applied at one of four levels, or _scopes_: System, Virtual Instance, Site, or Widget. Settings made in System Settings are set at a system-scoped level. For scopes labeled other than System, the System Settings values act as defaults. In other words, they identify where the system-wide configuration is overridden. True system-scoped configurations (those under a category’s _System Scope_ heading) are not overridden anywhere.

![Some System Settings categories hold entries of multiple scopes.](https://resources.learn.liferay.com/images/dxp/latest/en/system-administration/configuring-liferay/understanding-configuration-scope/images/02.png)

### [Scope Definitions](https://learn.liferay.com/web/guest/w/dxp/system-administration/configuring-liferay/understanding-configuration-scope#scope-definitions) <a href="#scope-definitions" id="scope-definitions"></a>

**System Scope:** Configuration made for system scoped entries are the final value for the application in a system-wide fashion. They affect the whole system and are not overridden anywhere else.

IMPORTANT

Configurations for the System Scope are not available on Liferay SaaS.

![Some System Settings entries are system scoped.](https://resources.learn.liferay.com/images/dxp/latest/en/system-administration/configuring-liferay/understanding-configuration-scope/images/01.png)

**Virtual Instance Scope:** Configuration at the Virtual Instance level is overridden in Instance Settings.

![Some System Settings are virtual instance scoped.](https://resources.learn.liferay.com/images/dxp/latest/en/system-administration/configuring-liferay/understanding-configuration-scope/images/05.png)

**Site Scope:** Configuration at this scope is overridden in each Site.

![Some System Settings are Site scoped.](https://resources.learn.liferay.com/images/dxp/latest/en/system-administration/configuring-liferay/understanding-configuration-scope/images/03.png)

**Widget Scope:** Configuration at this scope is overridden in each Widget Instance.

![Some System Settings entries are widget scoped.](https://resources.learn.liferay.com/images/dxp/latest/en/system-administration/configuring-liferay/understanding-configuration-scope/images/04.png)

### [Example: Configuring and Overriding Blogs Comments](https://learn.liferay.com/web/guest/w/dxp/system-administration/configuring-liferay/understanding-configuration-scope#example-configuring-and-overriding-blogs-comments) <a href="#example-configuring-and-overriding-blogs-comments" id="example-configuring-and-overriding-blogs-comments"></a>

If a configuration changed in System Settings is also configurable at a different scope, the System Settings value is a default that can be overridden. Once a configuration change is made at a more granular scope, a change at the system level does nothing.

For example, allowing comments is configurable for each Blogs widget.

1. Set the default behavior at Control Panel → Configuration → System Settings → Blogs (in the Content and Data category).
2. In the Blogs Entry under Widget Scope, disable the _Enable Comments_ checkbox.
3. Add a Blog Entry to a Site’s Content & Data → Blogs application.
4. Go to a page and add the Blogs Widget to the page.
5. Click the Options button (![Options](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-app-options.png)) for the widget and select _Configuration_.

You’ll see the same Enable Comments checkbox, and its default is now false (unchecked). Checking the box in the Widget Configuration overrides the System Settings entry. Changing the System Settings configuration has no effect on this widget anymore.

![Override widget scoped System Settings from the widget itself.](https://resources.learn.liferay.com/images/dxp/latest/en/system-administration/configuring-liferay/understanding-configuration-scope/images/06.png)

### [System Settings and Instance Settings](https://learn.liferay.com/web/guest/w/dxp/system-administration/configuring-liferay/understanding-configuration-scope#system-settings-and-instance-settings) <a href="#system-settings-and-instance-settings" id="system-settings-and-instance-settings"></a>

Instance Settings is similar to System Settings, but configures applications and services at the Virtual Instance scope. Like System Settings, the configuration entries in Instance Settings have a _Reset to Default Values_ option. In Instance Settings, this removes the custom configuration you provided and makes the current value (whether default or custom) from the system scope the default at the virtual instance scope.
