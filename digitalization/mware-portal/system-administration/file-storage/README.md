# File Storage

All files uploaded to Liferay DXP are stored in the instance’s designated file store. This includes files uploaded using [Documents and Media](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/documents-and-media), as well as those uploaded using applications that support file attachments (e.g., [Message Boards](https://learn.liferay.com/web/guest/w/dxp/collaboration-and-social/collaboration-and-social-overview)). The file store can be hosted on the local machine, on a network mounted file system, in a database, or in the cloud.

IMPORTANT

If you are going to production, we highly recommend reviewing the various File Store configuration options and choosing the one that best fits your needs, **before** going live. Doing so can avoid painful file store migrations later in a project’s life.

NOTE

The file store is also known as the Document Library.

### [Configuring Advanced File System Store](https://learn.liferay.com/web/guest/w/dxp/system-administration/file-storage#configuring-advanced-file-system-store) <a href="#configuring-advanced-file-system-store" id="configuring-advanced-file-system-store"></a>

The Advanced File System Store programmatically creates a folder structure that can expand to millions of files, by alphabetically nesting the files in folders. This allows more files to be stored and helps to avoid the limitation that some operating system have on the number of files that can be stored per folder. Storing fewer files per folder also improves file lookup performance.

![Advanced File System Store folder structure](https://resources.learn.liferay.com/images/dxp/latest/en/system-administration/file-storage/images/01.png)

To use the Advanced File System store method, following these steps:

1.  Configure [`portal-ext.properties`](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/reference/portal-properties) with this property:

    ```properties
    dl.store.impl=com.liferay.portal.store.file.system.AdvancedFileSystemStore
    ```
2. Restart Liferay.
3. In the Control Panel, navigate to _Configuration_ → _System Settings_ → _File Storage_.
4.  In the _Advanced File System Store_ screen, set the _Root Directory_ to your [Liferay Home](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/reference/liferay-home) path (absolute or relative).

    ![Advanced File System Store screen](https://resources.learn.liferay.com/images/dxp/latest/en/system-administration/file-storage/images/02.png)
5. Click _Save_.

Liferay is now saving files using Advanced File System Store.

IMPORTANT

Optionally [enable automatic antivirus scanning for uploaded files](https://learn.liferay.com/web/guest/w/dxp/system-administration/file-storage/enabling-antivirus-scanning-for-uploaded-files).

#### [File Storage in a Clustered Environment](https://learn.liferay.com/web/guest/w/dxp/system-administration/file-storage#file-storage-in-a-clustered-environment) <a href="#file-storage-in-a-clustered-environment" id="file-storage-in-a-clustered-environment"></a>

In a [clustered environment](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/setting-up-liferay/clustering-for-high-availability), point the store to a network mounted file system that all the nodes can access. The networked file system must be accessible to all nodes, support concurrent requests, and support file locking. If Advanced File System Store is used without such a file system and multiple users attempt writing to the same file concurrently, data corruption can occur.

### [Other File Storage Topics](https://learn.liferay.com/web/guest/w/dxp/system-administration/file-storage#other-file-storage-topics) <a href="#other-file-storage-topics" id="other-file-storage-topics"></a>

#### [Other File Storage Methods](https://learn.liferay.com/web/guest/w/dxp/system-administration/file-storage#other-file-storage-methods) <a href="#other-file-storage-methods" id="other-file-storage-methods"></a>

There are other built-in file storage methods available.

* [Simple File System Store](https://learn.liferay.com/web/guest/w/dxp/system-administration/file-storage/other-file-store-types/simple-file-system-store) uses the file system (local or a mounted share) to store files. This is the _default_ file store.
* [S3 Store (Amazon Simple Storage Service)](https://learn.liferay.com/web/guest/w/dxp/system-administration/file-storage/other-file-store-types/amazon-s3-store) uses Amazon’s cloud-based storage.
* [DBStore (Database Storage)](https://learn.liferay.com/web/guest/w/dxp/system-administration/file-storage/other-file-store-types/dbstore) stores files to the DXP database as `blobs`. DBStore’s file size limit is 1 gigabyte. To store files larger than 1 gigabyte, use the Simple File System Store or the Advanced File System Store.

WARNING

File system based stores (Simple, Advanced, S3) do not have transaction rollback capability. If a database transaction rollback occurs in a Document Library, the transaction’s file system changes are not reversed. Inconsistencies between Document Library files and those in the file system store can occur and may require manual synchronization. All stores except [DBStore](https://learn.liferay.com/web/guest/w/dxp/system-administration/file-storage/other-file-store-types/dbstore) are vulnerable to this limitation.

#### [Migrating Files Across File Stores](https://learn.liferay.com/web/guest/w/dxp/system-administration/file-storage#migrating-files-across-file-stores) <a href="#migrating-files-across-file-stores" id="migrating-files-across-file-stores"></a>

The Data Migration utility moves files from one store option to another. For example, it can be used to migrate files from a Simple File System Store (the default store) to an Advanced File System Store to leverage performance and scalability benefits. See [File Store Migration](https://learn.liferay.com/web/guest/w/dxp/system-administration/file-storage/file-store-migration) for more information.
