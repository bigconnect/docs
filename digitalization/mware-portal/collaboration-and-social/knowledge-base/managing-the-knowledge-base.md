# Managing the Knowledge Base

With Knowledge Base, you can draft product documentation quickly and organize it into hierarchies that end users can navigate. Add folders and nested articles to create the desired structure and order. Additionally, manage article and folder permissions to ensure only intended users can access your content.

![Add folders and nested articles to create the desired structure and order.](https://resources.learn.liferay.com/images/dxp/latest/en/collaboration-and-social/knowledge-base/managing-the-knowledge-base/images/01.png)

### [Adding Folders](https://learn.liferay.com/web/guest/w/dxp/collaboration-and-social/knowledge-base/managing-the-knowledge-base#adding-folders) <a href="#adding-folders" id="adding-folders"></a>

1. Open the _Site Menu_ (![Site Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-menu.png)) and go to _Content and Data_ → _Knowledge Base_.
2. In the Folders and Articles section (![Folders and Articles](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-pages-tree.png)), click _Add_ (![Add Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png)) and select _Folder_.
3. Enter a name and description.
4.  Configure the folder’s permissions. By default, the View permission is assigned to the Guest role, meaning anyone can view the folder.

    ![Enter a name, description, and configure permissions.](https://resources.learn.liferay.com/images/dxp/latest/en/collaboration-and-social/knowledge-base/managing-the-knowledge-base/images/02.png)
5. Click _Save_.

### [Managing Folders and Articles](https://learn.liferay.com/web/guest/w/dxp/collaboration-and-social/knowledge-base/managing-the-knowledge-base#managing-folders-and-articles) <a href="#managing-folders-and-articles" id="managing-folders-and-articles"></a>

After adding folders and articles, you can drag and drop them in the left side panel to organize and order your documentation. You can also click Actions (![Actions Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) to access additional management options.

![Drag and drop folders and articles to organize and order your documentation.](https://resources.learn.liferay.com/images/dxp/latest/en/collaboration-and-social/knowledge-base/managing-the-knowledge-base/images/03.png)

NOTE

For 7.4 U71/GA71 and earlier versions, Knowledge Base articles include a priority value. This value appears beneath the article’s title and determines its order in the Knowledge Base Display widget. You can configure this behavior via [Knowledge Base System Settings](https://learn.liferay.com/web/guest/w/dxp/collaboration-and-social/knowledge-base/knowledge-base-system-settings).

#### [Folder Options](https://learn.liferay.com/web/guest/w/dxp/collaboration-and-social/knowledge-base/managing-the-knowledge-base#folder-options) <a href="#folder-options" id="folder-options"></a>

![Click the Actions button for a folder to access these management options.](https://resources.learn.liferay.com/images/dxp/latest/en/collaboration-and-social/knowledge-base/managing-the-knowledge-base/images/04.png)

**Edit** (![Edit](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-edit.png)): Update the article’s name and description.

**Import** (![Import](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-import.png)): Import a ZIP archive of Markdown files and image attachments. See [Importing Knowledge Base Articles](https://learn.liferay.com/web/guest/w/dxp/collaboration-and-social/knowledge-base/importing-knowledge-base-articles) for more information.

**Move** (![Move](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-move-folder.png)): Relocate the folder.

**Subscribe** (Home folder only): Get notified when Knowledge Base articles are created, updated, or deleted.

**Permissions** (![Permissions](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-permissions.png)): Assign resource permissions to different roles.

**Delete** (![Delete](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-app-trash.png)): Immediately delete the folder and its contents.

NOTE

Enable the LPS-188058 [release feature flag](https://learn.liferay.com/web/guest/w/dxp/system-administration/configuring-liferay/feature-flags#release-feature-flags) to add recycle bin support for Knowledge Base folders and articles. While enabled, deleting folders moves them and their contents to the Recycle Bin.

Additionally, you can click _Info_ (![Info Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-information.png)) to toggle the information panel for the current folder.

#### [Article Options](https://learn.liferay.com/web/guest/w/dxp/collaboration-and-social/knowledge-base/managing-the-knowledge-base#article-options) <a href="#article-options" id="article-options"></a>

![Click the Actions button for an article to access these management options.](https://resources.learn.liferay.com/images/dxp/latest/en/collaboration-and-social/knowledge-base/managing-the-knowledge-base/images/05.png)

**Edit** (![Edit](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-edit.png)): Change the folder’s name and description.

**Add Child Article** (![Page](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-document.png)): Create a sub-article.

**Expire** (![Time](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-time.png)): Mark an article as expired and notify reviewers, owners, and subscribers. Expired articles cannot be displayed in sites. See [Setting Review and Expiration Dates for Articles](https://learn.liferay.com/web/guest/w/dxp/collaboration-and-social/knowledge-base/managing-the-knowledge-base#setting-review-and-expiration-dates-for-articles) for more information

**Subscribe** (![Subscribe](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-bell.png)): Sign up to receive updates to the article. Users receive notifications for the article based on system mail and notification settings.

**History** (![History](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-date-time.png)): View and compare different versions of the article.

**Print** (![Print](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-print.png)): Open a dialogue to print the article.

**Move** (![Move](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-move-folder.png)): Relocate an article to a different folder or parent article.

**Permissions** (![Permissions](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-permissions.png)): Manage permissions for the article.

**Delete** (![Delete](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-app-trash.png)): Immediately delete the article along with its child articles.

NOTE

Enable the LPS-188058 [release feature flag](https://learn.liferay.com/web/guest/w/dxp/system-administration/configuring-liferay/feature-flags#release-feature-flags) to add recycle bin support for Knowledge Base folders and articles. While enabled, deleting an article moves it and its child articles to the Recycle Bin.

While viewing an article, you can also click _Info_ (![Info Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-information.png)) to view article details and versions.

### [Scheduling Articles for Publishing](https://learn.liferay.com/web/guest/w/dxp/collaboration-and-social/knowledge-base/managing-the-knowledge-base#scheduling-articles-for-publishing) <a href="#scheduling-articles-for-publishing" id="scheduling-articles-for-publishing"></a>

[Release Feature](https://learn.liferay.com/web/guest/w/dxp/system-administration/configuring-liferay/feature-flags#release-feature-flags)

When creating or editing an article, you can schedule it for publishing at a specific date and time. While scheduled, the article only appears in the Knowledge Base application and does not appear in site pages.

![Click Schedule Publication to set a specific date and time for publishing.](https://resources.learn.liferay.com/images/dxp/latest/en/collaboration-and-social/knowledge-base/managing-the-knowledge-base/images/06.png)

If you edit a scheduled article and click _Save_, Liferay saves the article as a draft and cancels the scheduled publication. Alternatively, if you click _Edit Schedule_, you can choose whether to publish the article immediately or schedule it for publishing at a future date and time.

![When editing a scheduled article, you can click Save or Edit Schedule.](https://resources.learn.liferay.com/images/dxp/latest/en/collaboration-and-social/knowledge-base/managing-the-knowledge-base/images/07.png)

To use this feature, enable the LPS-188058 release feature flag.

### [Setting Review and Expiration Dates for Articles](https://learn.liferay.com/web/guest/w/dxp/collaboration-and-social/knowledge-base/managing-the-knowledge-base#setting-review-and-expiration-dates-for-articles) <a href="#setting-review-and-expiration-dates-for-articles" id="setting-review-and-expiration-dates-for-articles"></a>

Sometimes Knowledge Base articles are only valid for a period of time. For these situations, you can set expiration and review dates for Knowledge Base articles to help insure the integrity of your documentation. While set, Liferay checks Knowledge Base articles at a set interval for any articles that have reached their expiration or review date. It then sends notifications to the reviewers, owners, and subscribers. When an expiration date is approaching, Liferay notifies users that the article is expiring soon.

By default, articles are set never to expire or require review, but you can change this while creating or editing an article.

![Set review and expiration dates for articles.](https://resources.learn.liferay.com/images/dxp/latest/en/collaboration-and-social/knowledge-base/managing-the-knowledge-base/images/08.png)

The default value is one year in the future, but you can select the desired date and hour. Users receive notifications for the article based on system mail and notification settings.

When an expiration date is reached, the Knowledge Base article cannot be used in sites. Users accessing the Knowledge Base article through a URL receive the 404 HTTP status. Additionally, an expired article cannot be modified unless the expiration date is set to a future value or expiration is disabled.

Reaching the review date does not affect the article. It only reminds users to review the article’s content.

#### [Configuring Review and Expiration Notifications](https://learn.liferay.com/web/guest/w/dxp/collaboration-and-social/knowledge-base/managing-the-knowledge-base#configuring-review-and-expiration-notifications) <a href="#configuring-review-and-expiration-notifications" id="configuring-review-and-expiration-notifications"></a>

1. Open the _Global Menu_ (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)), go to the _Control Panel_ tab, and click _System Settings_.
2.  Under Content and Data, click _Knowledge Base_.

    ![Set the check interval and when to notify users of articles expiring soon.](https://resources.learn.liferay.com/images/dxp/latest/en/collaboration-and-social/knowledge-base/managing-the-knowledge-base/images/09.png)
3. In the Service tab under System Scope, enter a check interval. This value determines the number of minutes between checks. By default, the value is 15 minutes.
4. Determine the number of weeks before expiration to send notification. Articles within this range are assigned an Expiring Soon label. The default value is 1 week.
5. Click _Save_.
6. Go to the _Service_ tab under Site Scope.
7. Enable/Disable review and expiration emails.
8.  Determine the content of review and expiration emails.

    ![Configure review and expiration emails.](https://resources.learn.liferay.com/images/dxp/latest/en/collaboration-and-social/knowledge-base/managing-the-knowledge-base/images/10.png)
9. Click _Save_.

### [Reviewing Article History](https://learn.liferay.com/web/guest/w/dxp/collaboration-and-social/knowledge-base/managing-the-knowledge-base#reviewing-article-history) <a href="#reviewing-article-history" id="reviewing-article-history"></a>

When viewing an article’s history, you can view version details, compare content, and revert to an earlier version.

![View, compare, and revert article versions.](https://resources.learn.liferay.com/images/dxp/latest/en/collaboration-and-social/knowledge-base/managing-the-knowledge-base/images/11.png)

To compare version content, select two articles and click _Compare Versions_.

![Compare changes between two versions of an article.](https://resources.learn.liferay.com/images/dxp/latest/en/collaboration-and-social/knowledge-base/managing-the-knowledge-base/images/12.png)

This highlights all additions, deletions, and formatting changes between versions.

\
