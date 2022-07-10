# Managing the Knowledge Base

The Knowledge Base app in Site Administration manages the Knowledge Base. To navigate to this app, open the Menu (![Menu](https://liferay-support.zendesk.com/hc/article\_attachments/360029910951/icon-menu.png)) then go to Site Administration (the menu for your site) → _Content & Data_ → _Knowledge Base_.

**Note:** To access Knowledge Base in Site Administration, a Role must have the permission _Knowledge Base_ → _Access in Site Administration_. To add or act on articles, folders, or suggestions, the site administrator must grant the appropriate permissions using the Permissions window in Knowledge Base.

![Figure 1: You can manage Knowledge Base articles, folders, and suggestions.](https://liferay-support.zendesk.com/hc/article\_attachments/360029926752/kb-admin-articles.png)

### Setting the Knowledge Base’s Options <a href="#setting-the-knowledge-bases-options" id="setting-the-knowledge-bases-options"></a>

At the top-right of the Knowledge Base app, the Options menu (![Options](https://liferay-support.zendesk.com/hc/article\_attachments/360029926732/icon-options.png)) contains these options:

**Subscribe:** Get notified when Knowledge Base articles are created, updated, or deleted.

**Home Folder Permissions:** Define detailed permissions for the Knowledge Base app. You can choose the Roles that can perform the following tasks:

* Add/delete articles, folders, and templates
* Change the Knowledge Base app’s permissions
* Subscribe to articles
* View templates and suggestions

**Export/Import:** Export or import the Knowledge Base app’s configuration.

**Configuration:** Configure email notifications for article subscriptions and suggestions. You can also make the Knowledge Base app’s articles available via RSS (enabled by default), and configure the RSS feed’s options.

![Figure 2: The Knowledge Base Apps options.](https://liferay-support.zendesk.com/hc/article\_attachments/360029910971/kb-admin-options.png)

### Managing Knowledge Base Articles <a href="#managing-knowledge-base-articles" id="managing-knowledge-base-articles"></a>

Each article also has a _priority_ value that determines its position in the Knowledge Base Display widget’s navigation. Each article’s priority value appears beneath the article’s title. The Knowledge Base Display widget’s navigation arranges articles in ascending priority. Priority 1 is the highest priority. The higher an article’s priority, the higher it appears in the navigation. Articles are assigned the next lowest priority by default. This behavior can be changed via Knowledge Base System Settings.

To assign articles a new priority value, follow these steps:

1. Select _Move_ from the Actions menu (![Actions](https://liferay-support.zendesk.com/hc/article\_attachments/360029910931/icon-actions.png)) next to the article.
2. Enter a new priority value for the article.
3. Click _Move_ to apply the new priority.

You can also organize articles with folders. Follow these steps to create a folder:

1. Click the Add button (![Add](https://liferay-support.zendesk.com/hc/article\_attachments/360029926712/icon-add.png)) and select _Folder_. This opens a form for creating the new folder.
2.  Enter a name and an optional description.

    By default, anyone can view the folder. You can manage this setting along with the other permissions in the form’s _Permissions_ section.
3. Click _Save_. The folder is then listed in a table in the Articles tab.

The text immediately below the _Filter and Order_ selector at the top of the app shows your position in the folder hierarchy. Click a folder’s name in the hierarchy to navigate to it. You can also move articles into folders and create child articles. Knowledge Base also supports nested folders.

![Figure 3: This screenshot uses a red box to highlight the text that indicates the current position in the folder hierarchy.](https://liferay-support.zendesk.com/hc/article\_attachments/360029910891/kb-admin-folder-hierarchy.png)

Each folder’s Actions menu (![Actions](https://liferay-support.zendesk.com/hc/article\_attachments/360029910931/icon-actions.png)) lets you perform the following actions on the folder:

**Edit:** Change the folder’s name and description.

**Move:** Relocate the folder under a new parent folder or update its priority.

**Delete:** Remove the folder and its articles from the Knowledge Base.

**Permissions:** Grant or revoke the following permissions: add an article to the folder, add a sub-folder to the folder, delete the folder, move the folder, set permissions on the folder, edit (update) the folder, and view the folder.

You can also delete multiple articles or folders at once. To do this, select the checkbox for each item that you want to delete and click the _Delete_ button (![Delete](https://liferay-support.zendesk.com/hc/article\_attachments/360029926772/icon-delete.png)) that appears in the Management Bar. You can also see the info for selected items by clicking the _Info_ button (![Info](https://liferay-support.zendesk.com/hc/article\_attachments/360029910911/icon-information.png)) in the Management Bar.
