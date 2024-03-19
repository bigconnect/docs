# File Store Migration

Files can be migrated from one file store type to another. Here are some migration scenarios:

* Migrating from Simple File System Store to Advanced File System Store for improved performance and scalability.
* Migrating from Simple, Advanced, or S3 to DBStore to avoid transaction rollback vulnerabilities.
* Migrating from DBStore to Advanced File System Store to accommodate files larger than 1 gigabyte.

The Data Migration utility in [Server Administration](https://learn.liferay.com/web/guest/w/dxp/system-administration/using-the-server-administration-panel) facilitates the migration.

WARNING

**Always** [back up](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/maintaining-a-liferay-installation/backing-up) your file store and database before migrating. Testing the migration process on backup copies is advised.

### [Migrating Files](https://learn.liferay.com/web/guest/w/dxp/system-administration/file-storage/file-store-migration#migrating-files) <a href="#migrating-files" id="migrating-files"></a>

Here are the migration steps:

1. Configure the new file store in _System Settings_ → _File Storage_.
2.  In _Server Administration_ → _Data Migration_, select the repository hook (store implementation class) and click _Execute_.

    ![Data Migration utility](https://resources.learn.liferay.com/images/dxp/latest/en/system-administration/file-storage/file-store-migration/images/01.png)
3. Make sure the data migrated correctly.
4.  Configure [`portal-ext.properties`](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/reference/portal-properties) to the target file store type, using one of these settings:

    ```properties
    dl.store.impl=com.liferay.portal.store.db.DBStore
    dl.store.impl=com.liferay.portal.store.file.system.AdvancedFileSystemStore
    dl.store.impl=com.liferay.portal.store.file.system.FileSystemStore
    dl.store.impl=com.liferay.portal.store.s3.S3Store
    ```
5. Restart DXP.

Your DXP instance is using the migrated files on the newly configured file store.

WARNING

Make sure to save your new store configuration in System Settings. Otherwise, you can’t use the new store and following exception occurs: `java.lang.IllegalStateException: Store is not available`

\
