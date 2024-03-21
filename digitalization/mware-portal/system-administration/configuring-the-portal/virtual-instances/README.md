# Virtual Instances

MWARE Portal can host multiple “portals” on one installation. These are called Virtual Instances. Each Virtual Instance has completely separate data and configurations. Each is accessed via a unique domain name and for all intents and purposes, is a separate implementation that shares an installation with additional instances.

Configuration of a the portal system happens at different scopes. Configuration at the virtual instance scope is the next most broad scope after the system scope.

IMPORTANT

The system scope is the highest level configuration scope. All virtual instances are impacted by configuration done at this scope. The _instance_ scope applies only to one particular virtual instance.

If your installation contains only one virtual instance, there’s no practical difference between a system scoped configuration and a virtual instance scoped configuration. Both types of configuration apply throughout your system.

Perhaps you already have a server hosting a portal installation and a database. It has many Users, Sites, and specific instance settings. If you require a second similar installation, then adding a _Virtual Instance_ might be right for you.

Because of the unique domain name, visitors are directed to the correct Virtual Instance. Because Virtual Instances share an application server and OSGi container, they also share these customizations:

* All custom code you’ve deployed
* System-scoped configurations (e.g., `.config` files and changes made in _Control Panel_ → _Configuration_ → _System Settings_).
* Application server configuration.

You can add and manage Virtual Instances in _Control Panel_ → _Configuration_ → _Virtual Instances_.

![Add and manage virtual instances of the portal in the Control Panel's Virtual Instances section.](https://resources.learn.liferay.com/images/dxp/latest/en/system-administration/configuring-liferay/virtual-instances/understanding-virtual-instances/images/01.png)

You can configure an instance in _Control Panel_ → _Configuration_ → _Instance Settings_. The Instance Settings are organized into three sections:

* Platform
* Security
* Content and Data

This section of articles focuses on the _Platform_ settings.

![Instance Settings has several Platform categories.](https://resources.learn.liferay.com/images/dxp/latest/en/system-administration/configuring-liferay/virtual-instances/understanding-virtual-instances/images/02.png)
