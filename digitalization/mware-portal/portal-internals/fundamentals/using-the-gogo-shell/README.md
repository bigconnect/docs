# Using the Gogo Shell

The Gogo shell provides a way to interact with the module framework. Among other things, you can

* Dynamically install/uninstall bundles (modules)
* Examine package dependencies
* Examine extension points
* List service references

The control panel is the safest, most secure way to access Gogo shell:

1. Open the _Global Menu_ (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)), and select the _Control Panel_ tab.
2.  Click _Gogo Shell_ in the System section.

    The Gogo shell command screen appears.

    ![Gogo shell in the control panel](https://resources.learn.liferay.com/images/dxp/latest/en/liferay-internals/fundamentals/using-the-gogo-shell/images/02.png)

    NOTE

    In DXP 7.2, open the Control Panel and navigate to _Configuration_ → _Gogo Shell_
3.  Enter a Gogo shell command in the _g!_ text field. For example, enter `lb | grep "Liferay Announcements API"` to list information about the Announcements API bundle.

    NOTE

    If you have a cluster, you must run Gogo shell commands on all nodes. Commands run in the Control Panel’s Gogo shell only affect the current node.
4.  Click _Execute_.

    The command result output appears.

    ![The output section shows the command result.](https://resources.learn.liferay.com/images/dxp/latest/en/liferay-internals/fundamentals/using-the-gogo-shell/images/03.png)

WARNING

The Gogo shell is extremely powerful and can manipulate the platform’s core functionality. Only grant Gogo shell access to trusted administrators. Please see [Understanding Roles and Permissions](https://learn.liferay.com/web/guest/w/dxp/users-and-permissions/roles-and-permissions/understanding-roles-and-permissions) for more information.

\
