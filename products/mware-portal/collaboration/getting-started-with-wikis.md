# Getting Started with Wikis

The Menu (![Menu](https://liferay-support.zendesk.com/hc/article\_attachments/360029910431/icon-menu.png)) is the best place to start working with your wikis. Click the _Menu_ (![Menu](https://liferay-support.zendesk.com/hc/article\_attachments/360029910431/icon-menu.png)), navigate to your site, and select the _Content & Data_ section. If you’re updating an existing page-scoped wiki app instance, you can select that page scope from the scope menu the Gear icon (![Gear](https://liferay-support.zendesk.com/hc/article\_attachments/360029910471/icon-control-menu-gear.png)) makes available. The site’s wiki app instance is available in the Default scope. Once you’re in the proper content scope, click _Wiki_. The Wiki administration screen lets you add, modify, and delete wiki nodes. A Wiki app instance can contain many wiki nodes. By default, it contains one node: _Main_.

![Figure 1: The Wiki app instance has a wiki node named Main with a single front page. You can build on the Main node or click the Add icon to create a new node.](https://liferay-support.zendesk.com/hc/article\_attachments/360029926192/wiki-admin-empty.png)

### Configuring Wikis <a href="#configuring-wikis" id="configuring-wikis"></a>

Before adding to your wiki instance, you should configure it. The instance’s interfaces for permissions, export and import, configuration, and application templates are accessible from the Options menu. Click the _Options_ icon (![Options](https://liferay-support.zendesk.com/hc/article\_attachments/360029926172/icon-options.png)) to open this menu.

The following options are available in this menu:

**Wikis Permissions**: Specify which Roles can create wiki nodes and access the Wikis Permissions screen. For example, if you’ve created a specific Role for creating wiki nodes and want to enable that Role to create new wiki nodes in this wiki application instance, select the Role’s check box in the _Add Node_ column and then click _Save_.

**Export / Import**: Import existing wiki content into your wiki app instance, or export wiki content to a file.

**Configuration**: Configure email notifications and RSS feeds. The _Email From_, _Page Added Email_, and _Page Updated Email_ tabs are similar to other apps’ notification email settings tabs; they customize who wiki emails come from and the format and text of the email sent when a page is added or updated. The _RSS_ tab lets you configure RSS feeds.

### Adding Wikis <a href="#adding-wikis" id="adding-wikis"></a>

Follow these steps to create a new wiki node:

1. Click the _Add_ icon (![Add](https://liferay-support.zendesk.com/hc/article\_attachments/360029910391/icon-add.png)) to start creating a new wiki node. The _New Wiki Node_ form appears.
2. Add a name and description for the wiki node.
3.  Open the form’s _Permissions_ section and define the wiki node’s permissions. You can select the following permissions in the _Viewable by_ menu:

    * Anyone (Guest Role)
    * Site Members
    * Owner

    You can also click the _More Options_ link to assign permissions to specific Roles.
4. Click _Save_ when you’re done creating the wiki node.

![Figure 2: The New Wiki Node form lets you describe your new node, set view permissions, and set permissions for the Guest and Site Member roles.](https://liferay-support.zendesk.com/hc/article\_attachments/360029910451/wiki-new-wiki-node.png)

### Wiki Node Options <a href="#wiki-node-options" id="wiki-node-options"></a>

Next to each listed wiki node is an _Actions_ menu (![Actions](https://liferay-support.zendesk.com/hc/article\_attachments/360029910411/icon-actions.png)). Here are the actions available in this menu:

**Edit**: Edit the wiki’s name and description.

**Permissions**: Specify which roles can add attachments to wiki pages, add pages, delete pages, import pages, set permissions on the wiki node, subscribe to modifications, update existing pages, and view the wiki node.

**Import Pages**: Import data from other wikis. This lets you migrate from another wiki application to the portal wiki. You might want to do this if you’re migrating your site from a set of disparate applications (i.e., a separate forum, a separate wiki, a separate content management system) to thw portal, which provides all of these features. Currently, MediaWiki is the only supported wiki.

**RSS**: Subscribe to an RSS feed using Live Bookmarks, Yahoo, Microsoft Outlook, or an application on your machine.

**Subscribe**: Subscribe to a wiki node. Any time a wiki page is added or updated, the portal sends you an email notification.

**View Removed Attachments**: Display attachments that have been removed from the wiki node.

**Move to Recycle Bin**: Moves the wiki node to the [Recycle Bin](https://help.liferay.com/hc/zh-cn/articles/360028820732-Restoring-Deleted-Assets).

![Figure 3: Each wiki nodes Actions menu lists actions you can perform.](https://liferay-support.zendesk.com/hc/article\_attachments/360029926212/wiki-options.png)

Before opening wiki nodes to contributors, you should consider whether to associate a workflow with them. For example, you could create a workflow that requires an administrator’s approval to publish a wiki page modification (add, update, or delete). You can access your site’s default _Wiki Page_ workflow from within the Site Administration Menu, by navigating to _Configuration_ → _Workflow_ for your site.&#x20;
