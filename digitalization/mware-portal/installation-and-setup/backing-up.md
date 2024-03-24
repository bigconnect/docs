# Backing Up

Once you have your portal installation running, you should implement a comprehensive backup plan that accounts for unforeseen events. Backup copies are also valuable for testing updates (e.g., upgrades, new plugins, and more).

When considering a back up plan for the portal, we recommend backing up the following areas:

* Home directory
* Application Server
* Database
* Search Indexes
* Source Code

NOTE

Cloud automates and simplifies the process of creating and managing backups of the Document Library and the portal database. Because Cloud uses a Git-based DevOps pipeline, all configurations for the portal are also automatically backed up as well. To learn more, see the Cloud Backup Service Overview for more information.

### Home <a href="#liferay-home" id="liferay-home"></a>

The Home folder is important to back up because it contains the following files:

* **Portal properties and system properties:** The Home folder stores portal properties files (e.g., `portal-ext.properties`, `portal-setup-wizard.properties`, etc.) and system properties files (e.g., `system-ext.properties`).
* **`/data` folder:** the system stores configuration files, search indexes, and cache information in Home’s `/data` folder. Note, the `/data/document_library` folder is the default storage configuration location for the Simple File System Store. The Advanced File System Store requires setting the storage location explicitly.
* **`/license` folder (Subscription):** Holds the activation key for the Enterprise Subscription.
* **`/osgi` folder:** The files that comprise the system’s OSGi runtime are stored in Home’s `/osgi` folder. It contains all of the app and module JAR files deployed to the system. The `/osgi` folder also contains other required JAR files, configuration files, and log files.
* **`/logs` folder:** Contains the system’s log files. If a problem occurs on the system, the log files provide information for determining what went wrong.

NOTE

If you are using a Docker image and are using custom portal and application server configuration files via a bind mount, back up those files (e.g., files in your `[host folder]/files` folder).

Using a source control repository such as Git, BitBucket, Subversion, or CVS, is a great way to back up your Home folder.

IMPORTANT

If you configured your File Store (Document Library) to a location other than a `[Home]/data` subfolder, back up that location.

### Application Server <a href="#application-server" id="application-server"></a>

The application server has the the system descriptors, deployments, and dependencies you might have customized. If you’ve customized the system’s `web.xml` file, for example, you should back it up because a the system patch’s `web.xml` always overwrites the existing one. Backing up your entire application server is suggested.

### Database <a href="#database" id="database"></a>

The system’s database is the central repository for all of the portal’s information. It’s the most important component to back up. You can back up the database live (if your database allows this) or by exporting (dumping) the database into a file and then backing up the exported file.

For example, [MySQL’s `mysqldump`](https://dev.mysql.com/doc/refman/5.7/en/using-mysqldump.html) utility exports the entire database and data into a large SQL file:

```bash
mysqldump --databases my-liferay-database > my-liferay-database-backup.sql
```

This file can then be backed up. On restoring the database you can import this file into the database to recreate the database state to that of the time you exported the database.

### File Store <a href="#file-store" id="file-store"></a>

Back up your file store (Document Library). When you upgrade to a new version of the system, you must either refer to your existing Document Library or copy the Document Library to your new system environment.

### Search Indexes <a href="#search-indexes" id="search-indexes"></a>

Always back up your search indexes. Although most portal data can be restored from the database by a full re-index, search indexes are used as primary storage by some applications. A failure to back up indexes can result in total data loss for these applications.

In addition, users with large data sets can avoid re-indexing all of their content and assets when restoring from a backup. Backing up search data is easiest to do if you have a separate Elasticsearch or Solr environment storing the search indexes. Follow the search engine’s backup/restore documentation for the details (for example, see Elasticsearch’s [Snapshot and Restore documentation](https://www.elastic.co/guide/en/elasticsearch/reference/7.x/snapshot-restore.html)).

### Source Code <a href="#source-code" id="source-code"></a>

If you have extended the system or have written any plugins, they should be stored in a source code repository such as Git or BitBucket. You should back up your source code repository on a regular basis to preserve ongoing work.

### Conclusion <a href="#conclusion" id="conclusion"></a>

Backing up and restoring the portal’s data is essential to reviving an installation, whether you’re recovering from unexpected system failure or performing a routine upgrade. Exercising good, consistent backup procedures on the areas covered here ensures that you can recover the important data your installation requires.
