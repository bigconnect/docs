# Sharing Documents with Other Users

Using Documents and Media app, users can share files directly with each other.

You can disable sharing. For instructions on this, see Enabling and Configuring Sharing.

When you share, you grant some of your own permissions for that file to the receiving user. There are some important caveats:

* You can only grant View, Comment, or Update permissions. For example, you can’t grant Delete or Override Checkout permissions even if you have those permissions on the file.
* You can only grant permissions you have on the file. For example, you can’t grant Update permission if you only have View and Comment permissions on the file.
* You must grant at least View permission.
* Traditional role-based permissions always take precedence over sharing permissions. So although sharing can extend permissions, it can’t remove those granted via Roles in the portal.
* By default, the Guest role has Add Discussion permission. This overlaps with the Comment permission in sharing. Therefore, all users can comment on a file regardless of whether the Comment permission was granted via sharing. You can change this by removing the Add Discussion permission from the Guest role.

Also, note that the receiving user must be part of the same instance, but doesn’t have to be a member of the same site.

### Sharing Files in Documents and Media <a href="#sharing-files-in-documents-and-media" id="sharing-files-in-documents-and-media"></a>

To share a file, you must own that file or be an administrator. There are two ways to share a file: through the Documents and Media widget on a page, or in Site Administration. If using the latter method, ensure you have access to Site Administration.

#### Sharing a File on a Site Page <a href="#sharing-a-file-on-a-site-page" id="sharing-a-file-on-a-site-page"></a>

1.  Navigate to the page where the Documents and Media widget has been deployed.

    ![Go to the page with the DM widget.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/publishing-and-sharing/managing-document-access/sharing-documents-with-other-users/images/01.png)
2. Click on the file to be shared (for example, `speech.txt`).
3.  Click the _Info_ button to expand the Info Panel.

    ![Clicking on the Info button expands the Info Panel where the Share button is located.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/publishing-and-sharing/managing-document-access/sharing-documents-with-other-users/images/02.png)
4. Click _Share_.
5. Enter the email address of the collaborator. You can enter multiple addresses.
6. To let receiving users share the file, check the _Allow the document to be shared with other users_ box.
7.  Select the file permissions to grant to receiving users. Select a file permission to grant to receiving users:

    * **Update:** View, comment, and update.
    * **Comments:** View and comment.
    * **View:** View only.

    ![Select the permissions to grant to receiving users.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/publishing-and-sharing/managing-document-access/sharing-documents-with-other-users/images/03.png)
8. Click the _Share_ button on the window pane.

The file has been shared.

#### Share a File Using Site Administration <a href="#share-a-file-using-site-administration" id="share-a-file-using-site-administration"></a>

1. To access Site Administration, open the _Menu_ (![Product Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-menu.png)) and click your site’s name.
2. Click _Content & Data_ → _Documents and Media_.
3. Navigate to the folder where the file to be shared is located (for example, the `HR` folder).
4.  Click the file’s _Actions_ (![Actions](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) and select _Share_.

    ![Click Actions → Share.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/publishing-and-sharing/managing-document-access/sharing-documents-with-other-users/images/04.png)
5. Enter the name or email address of the user you want to share the file with. To share the file with multiple users, enter each user’s email address in a comma delimited list.
6. To let receiving users also share the file, select _Allow the document to be shared with other users_.
7. Select the file permissions to grant to receiving users:
   * **Update:** View, comment, and update.
   * **Comments:** View and comment.
   * **View:** View only.
8. Click _Share_.

### Working with Shared Files <a href="#working-with-shared-files" id="working-with-shared-files"></a>

You can access files shared with you in three places:

1.  **The Documents and Media Library:** Files shared with you are visible in their existing Documents and Media locations. For example, if someone shares a file with you that resides in the Documents and Media library’s Home folder, you can access the file in that folder.

    ![You can view a shared file in the same folder where it was first uploaded.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/publishing-and-sharing/managing-document-access/sharing-documents-with-other-users/images/05.png)
2.  **The Notifications app:** When a file is shared with you, you get a notification. Clicking the notification takes you to the file in Documents and Media. For information on notifications, see Managing Notifications and Requests.

    ![The Notifications app contains the notifications that are sent when a user shares a file with you.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/publishing-and-sharing/managing-document-access/sharing-documents-with-other-users/images/06.png)
3.  **The Shared Content app:** You can access this app from your user menu. It lists all the content shared with you and the content you shared. Each file has an Actions button (![Actions](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) for performing permitted actions on the file (e.g., view, comment, update).

    ![The Shared Content app lists the files shared with you, and the files you shared.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/publishing-and-sharing/managing-document-access/sharing-documents-with-other-users/images/07.png)

### Managing Shared Files <a href="#managing-shared-files" id="managing-shared-files"></a>

After sharing a file, you can un-share it or modify its permissions on a per-user basis. This can only be done by Administrators, the file’s owner, or any user with Update permission and permission to share the file. You can take these actions from the file’s Info panel in Documents and Media. Follow these steps:

1. Click the file in Documents and Media, then click _Info_ (![Info](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-information.png)) at the top-right. The file’s Info panel slides out from the right.
2.  Click the _Manage Collaborators_ link. This shows a list of the users you shared the file with and their file permissions.

    ![Click Manage Collaborators to open up the list of users you shared the file with.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/publishing-and-sharing/managing-document-access/sharing-documents-with-other-users/images/08.png)
3.  Make any changes you want to the list of collaborators. To un-share the file with a user, click the `x` icon next to that user. You can also change the file permissions via the selector menu for each user.

    ![Remove collaborators or change their permissions.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/publishing-and-sharing/managing-document-access/sharing-documents-with-other-users/images/09.png)
4. Click _Save_ and close the dialog.

\
