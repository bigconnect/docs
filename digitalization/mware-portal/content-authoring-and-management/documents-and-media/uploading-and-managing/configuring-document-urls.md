# Configuring Document URLs

When a file is uploaded, the portal automatically generates a friendly URL for accessing it. This URL uses the following structure: `[server-address]/documents/d/[site's friendly url]/[file-friendly-URL]`. Unless specified during upload, the portal uses the file’s name for the friendly URL, but you can change this value at any time. Once set, you can access the latest version of the file at its friendly URL.

NOTE

For 7.4  and earlier versions, the portal generates file URLs using the following structure: `[server-address]/documents/[site-id]/[folder-id]/[file-name]/[UUID]`. If you’re updating, existing files continue to use their old URLs based on this pattern. If, however, a user updates an existing file, the portal converts its URL to use the new structure. Until updated, you can continue to access your files using their old URLs.

### Setting Friendly URLs for Documents <a href="#setting-friendly-urls-for-documents" id="setting-friendly-urls-for-documents"></a>

You can configure a file’s friendly URL manually during file upload or whenever editing the file.

NOTE

When uploading multiple files, each file’s friendly URL is generated automatically using its name.

Follow these steps to set a file’s friendly URL:

1. Open the _Site Menu_ (![Site Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu.png)), expand _Content & Data_, and go to _Documents and Media_.
2. Begin uploading or editing the desired file.
3.  Edit the _Friendly URL_ field.

    Each URL value must be unique per site. If a value is already in use, the portal automatically resolves the conflict by adding a numeric value to the end of the URL (e.g., `my-file`, `my-file-1`, `my-file-2`).

    ![When uploading or editing a file, you can change its friendly URL.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/uploading-and-managing/configuring-document-urls/images/01.png)
4. Click _Publish_.

Once saved, you can access the file at the designated URL (e.g., `http://[server-address]/documents/d/[site-friendly-url]/[file-friendly-URL]`).

### Friendly URL History <a href="#friendly-url-history" id="friendly-url-history"></a>

When you change a file’s friendly URL, the portal automatically stores its previous values. As long as old URLs remain in the file’s history, you can use them to access the latest version of the file, and other files in the same site cannot use them. To reuse an old URL for another file, you must remove it from the file’s friendly URL history.

NOTE

By default, friendly URL redirects are temporary (302), but you can set them to permanent (301) if needed. See Configuring Friendly URL Redirects for more information.

Follow these steps to view and manage retained URLs for a file:

1. Open the _Site Menu_ (![Site Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-product-menu.png)), expand _Content & Data_, and go to _Documents and Media_.
2. Click _Actions_ (![Actions Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) for the desired file and select _Edit_.
3.  Click _History_ (![History Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-history.png)) in the Friendly URL field.

    ![Click the History button for the Friendly URL field.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/uploading-and-managing/configuring-document-urls/images/02.png)
4. Click _Restore_ (![Restore Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-restore2.png)) to replace the current friendly URL with the selected URL.
5.  Click _Forget URL_ (![Forget URL Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-delete.png)) to remove the selected URL from the file’s history. Once removed, you can no longer use the URL to access the file.

    ![Click the Restore or Forget URL buttons.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/uploading-and-managing/configuring-document-urls/images/03.png)

Changes to the friendly URL history are saved automatically.

NOTE

Beginning with version 7.4, the display page template framework takes into account each file’s friendly URL history. This means users can continue to access a file’s display page using old friendly URLs, provided those URLs remain in the file’s friendly URL history. All URLs in the file’s friendly URL history are redirected automatically to the file’s current URL.

When the file is on the same site as its display page template, the file’s friendly URL is appended to the display page URL (i.e., `display-page-url/file-friendly-url`). However, to avoid duplicate URLs, if the file is in a different site or asset library from the template, the URL pattern also includes the friendly URL of the file’s site or asset library (i.e., `display-page-url/site-friendly-url/document-friendly-url`).

### Document Version URLs <a href="#document-version-urls" id="document-version-urls"></a>

All file friendly URLs lead to the latest version of the file. If you want to access the URL for previous versions, navigate to the file’s version history and copy the _download_ URL for the desired version.

![Copy the download URL for the desired version.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/uploading-and-managing/configuring-document-urls/images/04.png)
