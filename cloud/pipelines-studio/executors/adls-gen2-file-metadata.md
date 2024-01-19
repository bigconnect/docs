# ADLS Gen2 File Metadata

The ADLS Gen2 File Metadata executor changes file metadata, creates an empty file, or removes a file or directory in Azure Data Lake Storage Gen2 each time it receives an event. To perform these tasks in Azure Data Lake Storage Gen1, use the ADLS Gen1 File Metadata executor.&#x20;

Before you use the executor, you must perform some prerequisite tasks.

An executor can perform a single task. To perform more than one task, use additional executors.

Use the ADLS Gen2 File Metadata executor as part of an event stream. For example, you might use the executor to move a file or change file permissions after it receives a file closure event from the Azure Data Lake Storage Gen2 destination.

To change metadata, configure an expression that represents the location and name of the file to process, and then specify the changes you want to make. To create an empty file, specify the output location for the file, and optionally specify the owner, permissions, and ACLs for the file. To remove a file or directory, specify the location of the file or directory.

When necessary, you can configure advanced properties to pass to the underlying Hadoop file system.

You can also configure the executor to generate events for another event stream. For more information about dataflow triggers and the event framework, see Dataflow Triggers Overview.

For a solution that describes how to use a similar file metadata executor, see Managing Output Files.

### Prerequisites <a href="#ariaid-title2" id="ariaid-title2"></a>

Complete the following prerequisites before you configure the ADLS Gen2 File Metadata executor:

1.  If necessary, create a new Azure Active Directory application for Data Pipelines.

    For information about creating a new application, see the [Azure documentation](https://docs.microsoft.com/en-us/azure/active-directory/develop/howto-create-service-principal-portal).
2.  Ensure that the Azure Active Directory Data Pipelines application has the appropriate access control to perform the necessary tasks.

    The Data Pipelines application requires Read, Write, and Execute permissions to perform all possible tasks.

    For information about configuring Gen2 access control, see the [Azure documentation](https://docs.microsoft.com/en-us/azure/storage/blobs/data-lake-storage-access-control).
3. Retrieve information from Azure to configure the executor.

After you complete all of the prerequisite tasks, you can configure the executor.

#### Retrieve Authentication Information <a href="#ariaid-title3" id="ariaid-title3"></a>

The ADLS Gen2 File Metadata executor can use different methods to authenticate connections to Azure.

The authentication information required depends on the selected authentication method:OAuth with Service PrincipalConnections made with OAuth with Service Principal authentication require the following information:

*   Application ID - Application ID for the Azure Active Directory Data Pipelines application. Also known as the client ID.

    For information on accessing the application ID from the Azure portal, see the [Azure documentation](https://docs.microsoft.com/en-us/azure/active-directory/develop/howto-create-service-principal-portal#get-application-id-and-authentication-key).
*   Tenant ID - Tenant ID for the Azure Active Directory Data Pipelines application. Also known as the directory ID.

    For information on accessing the tenant ID from the Azure portal, see the [Azure documentation](https://docs.microsoft.com/en-us/azure/active-directory/develop/howto-create-service-principal-portal#get-tenant-id).
*   Application Key - Authentication key or client secret for the Azure Active Directory application. Also known as the client secret.

    For information on accessing the application key from the Azure portal, see the [Azure documentation](https://docs.microsoft.com/en-us/azure/active-directory/develop/howto-create-service-principal-portal#get-application-id-and-authentication-key).

Shared KeyConnections made with Shared Key authentication require the following information:

*   Account Shared Key - Shared access key that Azure generated for the storage account.

    For more information on accessing the shared access key from the Azure portal, see the [Azure documentation](https://docs.microsoft.com/en-us/azure/storage/common/storage-account-manage#access-keys).

### Related Event Generating Stages <a href="#ariaid-title4" id="ariaid-title4"></a>

Use the ADLS Gen2 File Metadata executor in the event stream of a pipeline. The ADLS Gen2 File Metadata executor is optimized to update file metadata for output files or whole files processed by the Azure Data Lake Storage Gen2 destination or another ADLS Gen2 File Metadata executor. However, you can use the executor in any logical way.

### Changing Metadata <a href="#ariaid-title5" id="ariaid-title5"></a>

You can configure the ADLS Gen2 File Metadata executor to change metadata for a file in Azure Data Lake Storage Gen2 after receiving an event. For example, you might use the executor to change file permissions after a destination closes a file.

The ADLS Gen2 File Metadata executor can change the following file metadata at the same time:

* File name
* File location
* File owner and group
* File permissions
* Access control lists (ACLs)

To change metadata, the Azure Active Directory application for Data Pipelines must have the required permission.

#### Specifying the File Path <a href="#ariaid-title6" id="ariaid-title6"></a>

When using the ADLS Gen2 File Metadata executor to change file metadata, specify an expression for the File Path property that provides an absolute path to the files you want to use.

Use the default file path expression, `${record:value('/filepath')}`, to update output files closed by the Azure Data Lake Storage Gen2 destination. The file closure event records generated by this destination include a `filepath` field that contains the location and name of the closed output files.

To update whole files that the Azure Data Lake Storage Gen2 destination has completed streaming, use the following expression:

```
${record:value('/targetFileInfo/path')}
```

The whole file processed event records from the destination include a `/targetFileInfo/path` field that contains the location and name of the processed whole files.

For more information about the event records generated by the Azure Data Lake Storage Gen2 destination, see Event Records.

#### Changing the File Name or Location <a href="#ariaid-title7" id="ariaid-title7"></a>

When using the ADLS Gen2 File Metadata executor to change file metadata, you can change the name or location of files after they close. To specify new file names and locations, you can enter constants or expressions. Use any expression that evaluates to the values that you want to use.

When needed, you can include file functions in expressions to specify part of the existing file path. File functions can return any part of a path, file name, or extension.Example for moving filesSay the Azure Data Lake Storage Gen2 destination writes JSON files to the following directory structure:

```
/server1/weblogs/<subdir>/<filename>
```

After the files are written, you want the ADLS Gen2 File Metadata executor to move the files to a different root directory. To move files you need to specify the new location for the files. So you configure the executor to move files to a different directory while still using the rest of the path, as follows:

```
/newDir/${file:pathElement(record:value('/filepath'),1)}/${file:pathElement(record:value('/filepath'),2)}/
```

This expression uses newDir as the new root directory, then uses two levels of subdirectories. Do not include file names when moving files.

Example for renaming filesSay you want to add the .json suffix to the original file name. To rename files, you need to specify the new name for the files, so you use the following expression:

```
${file:fileName(record:value('/filepath'))}.json
```

This expression returns the file name from the `filepath` field in the event record and adds `.json` to the file name, such as `<filename>.json`.If you wanted to strip the extension from written files, you could use the following expression in the New Name property:

```
${file:removeExtension(file:fileName(record:value('/filepath')))}
```

This expression returns the file name from the event record of the `filepath` field, strips the extension from the name, and uses the result as the new file name.

For more information about file functions, see File Functions.

#### Defining the Owner, Group, Permissions, and ACLs <a href="#ariaid-title8" id="ariaid-title8"></a>

When using the ADLS Gen2 File Metadata executor to change file metadata or create an empty file, you can define the file owner, group, file permissions, and the access control list (ACL).Important: When the executor changes permissions, it removes existing permissions and implements the requested permissions. The executor does not add permissions to the existing permissions, so be sure to configure permissions exactly how you want them.You can set permissions using any combination of the following methods:Define a new owner and groupYou can define the owner and group for files. When you use this option, you must enter both an owner and a group name.Set file permissions using the octal or symbolic formatsYou can set file permissions by entering the permissions you want to use in octal or symbolic format.For example, you can use the following octal format to make files read-only:

```
0444
```

You can alternatively use the following symbolic format to make files read-only:

```
-r--r--r-- 
```

To make them read-only for the user and the group, forbidding all access to other users, you could use either of the following formats:

```
0440

-r--r-----
```

Define ACLsYou can define the ACLs for files. When you define ACLs, note that Azure Data Lake Storage expects permissions defined for the user, group, and other. You can alternatively add permissions for additional users or groups.Use the following format to define ACLs:

```
user::<permissions>,group::<permissions>,other::<permissions>\
[,<user | group>:<user or group name:<permissions>]
```

Define permissions using the symbolic format, with r, w, x, or - representing the permission type.For example, the following ACLs allow read access for the user and group only:

```
user::r--,group::r--,other::–-
```

If you wanted to allow read access to the operations group in addition to the group associated with the file, you would enter the following permissions:

```
user::r--,group::r--,other::–-,group:operations:r--
```

### Creating an Empty File <a href="#ariaid-title9" id="ariaid-title9"></a>

You can configure the ADLS Gen2 File Metadata executor to create empty files in Azure Data Lake Storage Gen2 upon receiving an event. You might create empty files to trigger downstream actions in other applications, such as Oozie.

To create an empty file, specify an expression for the File Path property that provides an absolute path to the location where you want the file created.Note: In most cases, you will not want to use the default expression. The default expression is more appropriate for changing file metadata.When creating an empty file, you can also specify the following file details:

* File owner and group
* File permissions
* Access control lists (ACLs)

For more information, see Defining the Owner, Group, Permissions, and ACLs.

### Removing a File or Directory <a href="#ariaid-title10" id="ariaid-title10"></a>

You can configure the ADLS Gen2 File Metadata executor to remove a file or directory from Azure Data Lake Storage Gen2 after receiving an event.

For example, say you run a daily pipeline that writes data to Azure Data Lake Storage Gen2. You can use the ADLS Gen2 File Metadata executor to remove the target directory and all of its contents before a pipeline starts processing data. Simply configure the pipeline to pass the pipeline start event to an ADLS Gen2 File Metadata executor, then specify the target directory when you configure the executor. For more information about using pipeline events, see Pipeline Event Generation.

Remove directories with caution. The executor removes directories recursively, deleting any subdirectories and their contents in addition to the specified directory.

### Event Generation <a href="#ariaid-title11" id="ariaid-title11"></a>

The ADLS Gen2 File Metadata executor can generate events that you can use in an event stream. When you enable event generation, the executor generates events each time it changes file metadata, creates an empty file, or removes a file or directory.

ADLS Gen2 File Metadata events can be used in any logical way. For example:

* With the Email executor to send a custom email after receiving an event.
* With a destination to store event information.

#### Event Records <a href="#ariaid-title12" id="ariaid-title12"></a>

Event records generated by the ADLS Gen2 File Metadata executor have the following event-related record header attributes. Record header attributes are stored as String values.

| Record Header Attribute       | Description                                                                                                                                                                                                                                                                                                                                          |
| ----------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| sdc.event.type                | <p>Event type. Uses the following event types:</p><ul><li>file-changed - Generated when the executor changes file metadata, including file name, location, permissions, or ACLs.</li><li>file-created - Generated when the executor creates an empty file.</li><li>file-removed - Generated when the executor removes a file or directory.</li></ul> |
| sdc.event.version             | Integer that indicates the version of the event record type.                                                                                                                                                                                                                                                                                         |
| sdc.event.creation\_timestamp | Epoch timestamp when the stage created the event.                                                                                                                                                                                                                                                                                                    |

The ADLS Gen2 File Metadata executor can generate the following types of event records:File changed

The executor generates a file-changed event record when it changes file metadata, including file name, location, permissions, or ACLs.

File-changed event records have the `sdc.event.type` record header attribute set to `file-changed` and include the following fields:

| Event Field Name | Description                                    |
| ---------------- | ---------------------------------------------- |
| filepath         | Most recent path and name of the changed file. |
| filename         | Most recent name of the changed file.          |

File created

The executor generates a file-created event record when it creates an empty file.

File-created event records have the `sdc.event.type` record header attribute set to `file-created` and include the following fields:

| Event Field Name | Description                          |
| ---------------- | ------------------------------------ |
| filepath         | Location where the file was created. |
| filename         | Name of the file.                    |

File removed

The executor generates a file-removed event record when it removes a file or directory.

File-removed event records have the `sdc.event.type` record header attribute set to `file-removed` and include the following fields:

| Event Field Name | Description                                                                                      |
| ---------------- | ------------------------------------------------------------------------------------------------ |
| filepath         | Location of the directory that was removed, or the directory where the removed file was located. |
| filename         | Name of the file that was removed, when applicable.                                              |
