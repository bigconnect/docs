# Virtual Instances

Here’s a quick scenario: you already have a server hosting a MWARE Portal installation and a database. It has many Users, Sites, and specific instance settings. If you require a second similar installation, then adding a _Virtual Instance_ might be right for you.

You can run more than one Virtual Instance on a single server with a shared database, but separate data and configurations. Users are directed to the correct Virtual Instance via its unique domain name. And because Virtual Instances share an application server and OSGi container, they also share these customizations:

* Custom code deployed by developers and administrators.
* System-scoped configurations (e.g., `.config` files, changes made in _Control Panel_ → _Configuration_ → _System Settings_).
* Application server configuration.

Administrators can manage Virtual Instances in _Control Panel_ → _Configuration_ → _System Settings_.

Follow these steps to create a Virtual Instance:

1. Before you create a Virtual Instance, configure its domain name in your network.
2. Go to _Control Panel_ → _Configuration_ → _Virtual Instances_.
3. Click the _Add_ button (![Add](https://liferay-support.zendesk.com/hc/article\_attachments/360030187032/icon-add.png)). This opens the _New Instance_ form.
4. Complete the New Instance form as follows:

**Web ID:** The instance’s ID. Using the domain name is a common convention.

**Virtual Host:** The domain name you configured in your network. When users are directed to your server via this domain name, they’ll be sent to the Virtual Instance that contains their data.

**Mail Domain:** The mail host’s domain name for the Virtual Instance. Email notifications are sent from the instance using this domain.

**Max Users:** The maximum number of user accounts the Virtual Instance supports. Enter _0_ to support unlimited users.

**Active:** Whether the Virtual Instance is active. Note that inactive Virtual Instances aren’t accessible to anyone, even the administrator.

Click _Save_ when you’re done filling out the form.

Now you can navigate to the instance using its domain name. You’re brought to what looks like a clean install of MWARE Portal. This is your new Virtual Instance! You can configure it any way you like. The remaining articles in this section show you how to configure an instance’s settings.
