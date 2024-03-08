# Managing Document Changes with Checkout

When you check out a document in the Document Library, only you can make changes to it until you check it back in. This prevents conflicting edits on the same document by multiple users. When you check out a file, you can download it, replace it, move it to another Document Library folder, check it in, or cancel the checkout. Checking in a file also increments its version, which tracks changes.

Follow these steps to checkout and then check-in a Document Library file:

1.  Checkout the file by clicking its Actions icon (![Actions](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) → _Checkout_. Upon checkout, the file’s status changes to Draft and a lock icon appears on the file.

    ![The file on the right in this image is checked out.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/publishing-and-sharing/managing-document-access/managing-document-changes-with-checkout/images/01.png)
2. Download the file by clicking its Actions icon (![Actions](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) → _Download_.
3. Edit the file locally.
4. Return to the Documents and Media Library and click the file’s Actions icon (![Actions](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) → _Edit_. The file’s edit screen appears.
5. From the file’s Edit screen, select the edited local file for upload.
6. Click _Save and Check In_. In the pop-up that appears, select whether your change is a major or minor version, add any version notes that you need, and click _Save_.

TIP

If you edit a file without checking it out, the file’s edit screen displays a toggle for _Customize the Version Number Increment and Describe My Changes_. Setting this to _YES_ lets you specify the version increment’s type and description.

### [Viewing the File’s Version History](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/documents-and-media/publishing-and-sharing/managing-document-access/managing-document-changes-with-checkout#viewing-the-files-version-history) <a href="#viewing-the-files-version-history" id="viewing-the-files-version-history"></a>

Follow these steps to access a file’s version history:

1. Click the file in the Documents and Media Library.
2. Click the file’s _Info_ button (![i](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-information.png)) at the top-right of the screen. This opens the file’s info panel.
3.  Select the _Versions_ tab in the info panel.

    ![Liferay keeps track of the version history.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/publishing-and-sharing/managing-document-access/managing-document-changes-with-checkout/images/02.png)

Each file version has an Actions menu (![Actions](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) that you can use to perform the following actions on that file version:

| Action             | Description                                                                                                                        |
| ------------------ | ---------------------------------------------------------------------------------------------------------------------------------- |
| **Download**       | Download the selected version of the file to your machine.                                                                         |
| **View**           | View the file entry screen for the selected version of the file.                                                                   |
| **Revert**         | Restores the selected file version as a new major file version. Note that this option isn’t available for the newest file version. |
| **Delete Version** | Remove the file version from the Document Library. All other file versions remain intact.                                          |

### [Using WebDAV](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/documents-and-media/publishing-and-sharing/managing-document-access/managing-document-changes-with-checkout#using-webdav) <a href="#using-webdav" id="using-webdav"></a>

You can manage files using WebDAV and continue to track version history. To learn more, see [Accessing Documents with WebDAV](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/documents-and-media/publishing-and-sharing/accessing-documents-with-webdav).

\
