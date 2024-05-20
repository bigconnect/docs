# Security

## HDFS Permissions

### Overview

The Hadoop Distributed File System (HDFS) implements a permissions model for files and directories that shares much of the POSIX model. Each file and directory is associated with an owner and a group. The file or directory has separate permissions for the user that is the owner, for other users that are members of the group, and for all other users. For files, the r permission is required to read the file, and the w permission is required to write or append to the file. For directories, the r permission is required to list the contents of the directory, the w permission is required to create or delete files or directories, and the x permission is required to access a child of the directory.

In contrast to the POSIX model, there are no setuid or setgid bits for files as there is no notion of executable files. For directories, there are no setuid or setgid bits directory as a simplification. The sticky bit can be set on directories, preventing anyone except the superuser, directory owner or file owner from deleting or moving the files within the directory. Setting the sticky bit for a file has no effect. Collectively, the permissions of a file or directory are its mode. In general, Unix customs for representing and displaying modes will be used, including the use of octal numbers in this description. When a file or directory is created, its owner is the user identity of the client process, and its group is the group of the parent directory (the BSD rule).

HDFS also provides optional support for POSIX ACLs (Access Control Lists) to augment file permissions with finer-grained rules for specific named users or named groups. ACLs are discussed in greater detail later in this document.

Each client process that accesses HDFS has a two-part identity composed of the user name, and groups list. Whenever HDFS must do a permissions check for a file or directory foo accessed by a client process,

* If the user name matches the owner of foo, then the owner permissions are tested;
* Else if the group of foo matches any of member of the groups list, then the group permissions are tested;
* Otherwise the other permissions of foo are tested.

If a permissions check fails, the client operation fails.

### User Identity <a href="#user_identity" id="user_identity"></a>

As of Hadoop 0.22, Hadoop supports two different modes of operation to determine the user’s identity, specified by the hadoop.security.authentication property:

*   simple

    In this mode of operation, the identity of a client process is determined by the host operating system. On Unix-like systems, the user name is the equivalent of \`whoami\`.
*   kerberos

    In Kerberized operation, the identity of a client process is determined by its Kerberos credentials. For example, in a Kerberized environment, a user may use the `kinit` utility to obtain a Kerberos ticket-granting-ticket (TGT) and use klist to determine their current principal. When mapping a Kerberos principal to an HDFS username, all components except for the primary are dropped. For example, a principal [todd/foobar@CORP.COMPANY.COM](mailto:todd/foobar@CORP.COMPANY.COM) will act as the simple username todd on HDFS.

Regardless of the mode of operation, the user identity mechanism is extrinsic to HDFS itself. There is no provision within HDFS for creating user identities, establishing groups, or processing user credentials.

### Group Mapping <a href="#group_mapping" id="group_mapping"></a>

Once a username has been determined as described above, the list of groups is determined by a group mapping service, configured by the `hadoop.security.group.mapping` property. See [Hadoop Groups Mapping](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/GroupsMapping.html) for details.

### Permission Checks <a href="#permission_checks" id="permission_checks"></a>

Each HDFS operation demands that the user has specific permissions (some combination of READ, WRITE and EXECUTE), granted through file ownership, group membership or the other permissions. An operation may perform permission checks at multiple components of the path, not only the final component. Additionally, some operations depend on a check of the owner of a path.

All operations require traversal access. Traversal access demands the EXECUTE permission on all existing components of the path, except for the final path component. For example, for any operation accessing `/foo/bar/baz`, the caller must have EXECUTE permission on `/`, `/foo` and `/foo/bar`.

The following table describes the permission checks performed by HDFS for each component of the path.

* Ownership: Whether or not to check if the caller is the owner of the path. Typically, operations that change the ownership or permission metadata demand that the caller is the owner.
* Parent: The parent directory of the requested path. For example, for the path `/foo/bar/baz`, the parent is `/foo/bar`.
* Ancestor: The last existing component of the requested path. For example, for the path `/foo/bar/baz`, the ancestor path is `/foo/bar` if `/foo/bar` exists. The ancestor path is `/foo` if `/foo` exists but `/foo/bar` does not exist.
* Final: The final component of the requested path. For example, for the path `/foo/bar/baz`, the final path component is `/foo/bar/baz`.
* Sub-tree: For a path that is a directory, the directory itself and all of its child sub-directories, recursively. For example, for the path `/foo/bar/baz`, which has 2 sub-directories named `buz` and `boo`, the sub-tree is `/foo/bar/baz`, `/foo/bar/baz/buz` and `/foo/bar/baz/boo`.

| Operation             | Ownership | Parent          | Ancestor            | Final                               | Sub-tree             |
| --------------------- | --------- | --------------- | ------------------- | ----------------------------------- | -------------------- |
| append                | NO        | N/A             | N/A                 | WRITE                               | N/A                  |
| concat                | NO \[2]   | WRITE (sources) | N/A                 | READ (sources), WRITE (destination) | N/A                  |
| create                | NO        | N/A             | WRITE               | WRITE \[1]                          | N/A                  |
| createSnapshot        | YES       | N/A             | N/A                 | N/A                                 | N/A                  |
| delete                | NO \[2]   | WRITE           | N/A                 | N/A                                 | READ, WRITE, EXECUTE |
| deleteSnapshot        | YES       | N/A             | N/A                 | N/A                                 | N/A                  |
| getAclStatus          | NO        | N/A             | N/A                 | N/A                                 | N/A                  |
| getBlockLocations     | NO        | N/A             | N/A                 | READ                                | N/A                  |
| getContentSummary     | NO        | N/A             | N/A                 | N/A                                 | READ, EXECUTE        |
| getFileInfo           | NO        | N/A             | N/A                 | N/A                                 | N/A                  |
| getFileLinkInfo       | NO        | N/A             | N/A                 | N/A                                 | N/A                  |
| getLinkTarget         | NO        | N/A             | N/A                 | N/A                                 | N/A                  |
| getListing            | NO        | N/A             | N/A                 | READ, EXECUTE                       | N/A                  |
| getSnapshotDiffReport | NO        | N/A             | N/A                 | READ                                | READ                 |
| getStoragePolicy      | NO        | N/A             | N/A                 | READ                                | N/A                  |
| getXAttrs             | NO        | N/A             | N/A                 | READ                                | N/A                  |
| listXAttrs            | NO        | EXECUTE         | N/A                 | N/A                                 | N/A                  |
| mkdirs                | NO        | N/A             | WRITE               | N/A                                 | N/A                  |
| modifyAclEntries      | YES       | N/A             | N/A                 | N/A                                 | N/A                  |
| removeAcl             | YES       | N/A             | N/A                 | N/A                                 | N/A                  |
| removeAclEntries      | YES       | N/A             | N/A                 | N/A                                 | N/A                  |
| removeDefaultAcl      | YES       | N/A             | N/A                 | N/A                                 | N/A                  |
| removeXAttr           | NO \[2]   | N/A             | N/A                 | WRITE                               | N/A                  |
| rename                | NO \[2]   | WRITE (source)  | WRITE (destination) | N/A                                 | N/A                  |
| renameSnapshot        | YES       | N/A             | N/A                 | N/A                                 | N/A                  |
| setAcl                | YES       | N/A             | N/A                 | N/A                                 | N/A                  |
| setOwner              | YES \[3]  | N/A             | N/A                 | N/A                                 | N/A                  |
| setPermission         | YES       | N/A             | N/A                 | N/A                                 | N/A                  |
| setReplication        | NO        | N/A             | N/A                 | WRITE                               | N/A                  |
| setStoragePolicy      | NO        | N/A             | N/A                 | WRITE                               | N/A                  |
| setTimes              | NO        | N/A             | N/A                 | WRITE                               | N/A                  |
| setXAttr              | NO \[2]   | N/A             | N/A                 | WRITE                               | N/A                  |
| truncate              | NO        | N/A             | N/A                 | WRITE                               | N/A                  |

\[1] WRITE access on the final path component during `create` is only required if the call uses the overwrite option and there is an existing file at the path.

\[2] Any operation that checks WRITE permission on the parent directory also checks ownership if the [sticky bit](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html#Overview) is set.

\[3] Calling `setOwner` to change the user that owns a file requires [HDFS super-user](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html#The\_Super-User) access. HDFS super-user access is not required to change the group, but the caller must be the owner of the file and a member of the specified group.

### Understanding the Implementation <a href="#understanding_the_implementation" id="understanding_the_implementation"></a>

Each file or directory operation passes the full path name to the NameNode, and the permissions checks are applied along the path for each operation. The client framework will implicitly associate the user identity with the connection to the NameNode, reducing the need for changes to the existing client API. It has always been the case that when one operation on a file succeeds, the operation might fail when repeated because the file, or some directory on the path, no longer exists. For instance, when the client first begins reading a file, it makes a first request to the NameNode to discover the location of the first blocks of the file. A second request made to find additional blocks may fail. On the other hand, deleting a file does not revoke access by a client that already knows the blocks of the file. With the addition of permissions, a client’s access to a file may be withdrawn between requests. Again, changing permissions does not revoke the access of a client that already knows the file’s blocks.

### Changes to the File System API <a href="#changes_to_the_file_system_api" id="changes_to_the_file_system_api"></a>

All methods that use a path parameter will throw `AccessControlException` if permission checking fails.

New methods:

* `public FSDataOutputStream create(Path f, FsPermission permission, boolean overwrite, int bufferSize, short replication, long blockSize, Progressable progress) throws IOException;`
* `public boolean mkdirs(Path f, FsPermission permission) throws IOException;`
* `public void setPermission(Path p, FsPermission permission) throws IOException;`
* `public void setOwner(Path p, String username, String groupname) throws IOException;`
* `public FileStatus getFileStatus(Path f) throws IOException;`will additionally return the user, group and mode associated with the path.

The mode of a new file or directory is restricted by the umask set as a configuration parameter. When the existing `create(path, …)` method (without the permission parameter) is used, the mode of the new file is `0666 & ^umask`. When the new `create(path, permission, …)` method (with the permission parameter P) is used, the mode of the new file is `P & ^umask & 0666`. When a new directory is created with the existing `mkdirs(path)` method (without the permission parameter), the mode of the new directory is `0777 & ^umask`. When the new `mkdirs(path, permission)` method (with the permission parameter P) is used, the mode of new directory is `P & ^umask & 0777`.

### Changes to the Application Shell <a href="#changes_to_the_application_shell" id="changes_to_the_application_shell"></a>

New operations:

*   `chmod [-R] mode file ...`

    Only the owner of a file or the super-user is permitted to change the mode of a file.
*   `chgrp [-R] group file ...`

    The user invoking chgrp must belong to the specified group and be the owner of the file, or be the super-user.
*   `chown [-R] [owner][:[group]] file ...`

    The owner of a file may only be altered by a super-user.
* `ls file ...`
*   `lsr file ...`

    The output is reformatted to display the owner, group and mode.

### The Super-User <a href="#the_super-user" id="the_super-user"></a>

The super-user is the user with the same identity as the NameNode process itself. Loosely, if you started the NameNode, then you are the super-user. The super-user can do anything in that permissions checks never fail for the super-user. There is no persistent notion of who was the super-user; when the NameNode is started the process identity determines who is the super-user for now. The HDFS super-user does not have to be the super-user of the NameNode host, nor is it necessary that all clusters have the same super-user. Also, an experimenter running HDFS on a personal workstation, conveniently becomes that installation’s super-user without any configuration.

In addition, the administrator may identify a distinguished group using a configuration parameter. If set, members of this group are also super-users.

### The Web Server <a href="#the_web_server" id="the_web_server"></a>

By default, the identity of the web server is a configuration parameter. That is, the NameNode has no notion of the identity of the real user, but the web server behaves as if it has the identity (user and groups) of a user chosen by the administrator. Unless the chosen identity matches the super-user, parts of the name space may be inaccessible to the web server.

### ACLs (Access Control Lists) <a href="#acls_.28access_control_lists.29" id="acls_.28access_control_lists.29"></a>

In addition to the traditional POSIX permissions model, HDFS also supports POSIX ACLs (Access Control Lists). ACLs are useful for implementing permission requirements that differ from the natural organizational hierarchy of users and groups. An ACL provides a way to set different permissions for specific named users or named groups, not only the file’s owner and the file’s group.

By default, support for ACLs is enabled, and the NameNode allows creation of ACLs. To disable support for ACLs, set `dfs.namenode.acls.enabled` to false in the NameNode configuration.

An ACL consists of a set of ACL entries. Each ACL entry names a specific user or group and grants or denies read, write and execute permissions for that specific user or group. For example:

```
   user::rw-
   user:bruce:rwx                  #effective:r--
   group::r-x                      #effective:r--
   group:sales:rwx                 #effective:r--
   mask::r--
   other::r--
```

ACL entries consist of a type, an optional name and a permission string. For display purposes, ‘:’ is used as the delimiter between each field. In this example ACL, the file owner has read-write access, the file group has read-execute access and others have read access. So far, this is equivalent to setting the file’s permission bits to 654.

Additionally, there are 2 extended ACL entries for the named user bruce and the named group sales, both granted full access. The mask is a special ACL entry that filters the permissions granted to all named user entries and named group entries, and also the unnamed group entry. In the example, the mask has only read permissions, and we can see that the effective permissions of several ACL entries have been filtered accordingly.

Every ACL must have a mask. If the user doesn’t supply a mask while setting an ACL, then a mask is inserted automatically by calculating the union of permissions on all entries that would be filtered by the mask.

Running `chmod` on a file that has an ACL actually changes the permissions of the mask. Since the mask acts as a filter, this effectively constrains the permissions of all extended ACL entries instead of changing just the group entry and possibly missing other extended ACL entries.

The model also differentiates between an “access ACL”, which defines the rules to enforce during permission checks, and a “default ACL”, which defines the ACL entries that new child files or sub-directories receive automatically during creation. For example:

```
   user::rwx
   group::r-x
   other::r-x
   default:user::rwx
   default:user:bruce:rwx          #effective:r-x
   default:group::r-x
   default:group:sales:rwx         #effective:r-x
   default:mask::r-x
   default:other::r-x
```

Only directories may have a default ACL. When a new file or sub-directory is created, it automatically copies the default ACL of its parent into its own access ACL. A new sub-directory also copies it to its own default ACL. In this way, the default ACL will be copied down through arbitrarily deep levels of the file system tree as new sub-directories get created.

The exact permission values in the new child’s access ACL are subject to filtering by the mode parameter. Considering the default umask of 022, this is typically 755 for new directories and 644 for new files. The mode parameter filters the copied permission values for the unnamed user (file owner), the mask and other. Using this particular example ACL, and creating a new sub-directory with 755 for the mode, this mode filtering has no effect on the final result. However, if we consider creation of a file with 644 for the mode, then mode filtering causes the new file’s ACL to receive read-write for the unnamed user (file owner), read for the mask and read for others. This mask also means that effective permissions for named user bruce and named group sales are only read.

Note that the copy occurs at time of creation of the new file or sub-directory. Subsequent changes to the parent’s default ACL do not change existing children.

The default ACL must have all minimum required ACL entries, including the unnamed user (file owner), unnamed group (file group) and other entries. If the user doesn’t supply one of these entries while setting a default ACL, then the entries are inserted automatically by copying the corresponding permissions from the access ACL, or permission bits if there is no access ACL. The default ACL also must have mask. As described above, if the mask is unspecified, then a mask is inserted automatically by calculating the union of permissions on all entries that would be filtered by the mask.

Note that you can not have unlimited amount of ACL entries for a given file or directory. The maximum number is 32 for access and 32 for default entries which is 64 in total.

When considering a file that has an ACL, the algorithm for permission checks changes to:

* If the user name matches the owner of file, then the owner permissions are tested;
* Else if the user name matches the name in one of the named user entries, then these permissions are tested, filtered by the mask permissions;
* Else if the group of file matches any member of the groups list, and if these permissions filtered by the mask grant access, then these permissions are used;
* Else if there is a named group entry matching a member of the groups list, and if these permissions filtered by the mask grant access, then these permissions are used;
* Else if the file group or any named group entry matches a member of the groups list, but access was not granted by any of those permissions, then access is denied;
* Otherwise the other permissions of file are tested.

Best practice is to rely on traditional permission bits to implement most permission requirements, and define a smaller number of ACLs to augment the permission bits with a few exceptional rules. A file with an ACL incurs an additional cost in memory in the NameNode compared to a file that has only permission bits.

### ACLs File System API <a href="#acls_file_system_api" id="acls_file_system_api"></a>

New methods:

* `public void modifyAclEntries(Path path, List<AclEntry> aclSpec) throws IOException;`
* `public void removeAclEntries(Path path, List<AclEntry> aclSpec) throws IOException;`
* `public void public void removeDefaultAcl(Path path) throws IOException;`
* `public void removeAcl(Path path) throws IOException;`
* `public void setAcl(Path path, List<AclEntry> aclSpec) throws IOException;`
* `public AclStatus getAclStatus(Path path) throws IOException;`

### ACLs Shell Commands <a href="#acls_shell_commands" id="acls_shell_commands"></a>

*   `hdfs dfs -getfacl [-R] <path>`

    Displays the Access Control Lists (ACLs) of files and directories. If a directory has a default ACL, then getfacl also displays the default ACL.
*   `hdfs dfs -setfacl [-R] [-b |-k -m |-x <acl_spec> <path>] |[--set <acl_spec> <path>]`

    Sets Access Control Lists (ACLs) of files and directories.
*   `hdfs dfs -ls <args>`

    The output of `ls` will append a ‘+’ character to the permissions string of any file or directory that has an ACL.

### Configuration Parameters <a href="#configuration_parameters" id="configuration_parameters"></a>

*   `dfs.permissions.enabled = true`

    If yes use the permissions system as described here. If no, permission checking is turned off, but all other behavior is unchanged. Switching from one parameter value to the other does not change the mode, owner or group of files or directories. Regardless of whether permissions are on or off, chmod, chgrp, chown and setfacl always check permissions. These functions are only useful in the permissions context, and so there is no backwards compatibility issue. Furthermore, this allows administrators to reliably set owners and permissions in advance of turning on regular permissions checking.
*   `dfs.web.ugi = webuser,webgroup`

    The user name to be used by the web server. Setting this to the name of the super-user allows any web client to see everything. Changing this to an otherwise unused identity allows web clients to see only those things visible using “other” permissions. Additional groups may be added to the comma-separated list.
*   `dfs.permissions.superusergroup = supergroup`

    The name of the group of super-users.
*   `fs.permissions.umask-mode = 0022`

    The umask used when creating files and directories. For configuration files, the decimal value 18 may be used.
*   `dfs.cluster.administrators = ACL-for-admins`

    The administrators for the cluster specified as an ACL. This controls who can access the default servlets, etc. in the HDFS.
*   `dfs.namenode.acls.enabled = true`

    Set to true to enable support for HDFS ACLs (Access Control Lists). By default, ACLs are enabled. When ACLs are disabled, the NameNode rejects all attempts to set an ACL.
*   `dfs.namenode.posix.acl.inheritance.enabled`

    Set to true to enable POSIX style ACL inheritance. Enabled by default. When it is enabled and the create request comes from a compatible client, the NameNode will apply default ACLs from the parent directory to the create mode and ignore the client umask. If no default ACL is found, it will apply the client umask.

## Hadoop Groups Mapping

### Overview

The groups of a user is determined by a group mapping service provider. Hadoop supports various group mapping mechanisms, configured by the `hadoop.security.group.mapping` property. Some of them, such as `JniBasedUnixGroupsMappingWithFallback`, use operating systems’ group name resolution and requires no configuration. But Hadoop also supports special group mapping mechanisms through LDAP and composition of LDAP and operating system group name resolution, which require additional configurations. `hadoop.security.group.mapping` can be one of the following:

*   org.apache.hadoop.security.JniBasedUnixGroupsMappingWithFallback

    The default implementation. It will determine if the Java Native Interface (JNI) is available. If JNI is available, the implementation will use the API within hadoop to resolve a list of groups for a user. If JNI is not available then the shell-based implementation, `ShellBasedUnixGroupsMapping`, is used.
*   org.apache.hadoop.security.JniBasedUnixGroupsNetgroupMappingWithFallback

    Similar to `JniBasedUnixGroupsMappingWithFallback`. If JNI is available, it obtains netgroup membership using the Hadoop native API; otherwise uses `ShellBasedUnixGroupsNetgroupMapping`.
*   org.apache.hadoop.security.ShellBasedUnixGroupsMapping

    This implementation shells out with the `bash -c groups` command (for a Linux/Unix environment) or the `net group` command (for a Windows environment) to resolve a list of groups for a user.
*   org.apache.hadoop.security.ShellBasedUnixGroupsNetgroupMapping

    This implementation is similar to `ShellBasedUnixGroupsMapping`, except that it executes `getent netgroup` command to get netgroup membership.
*   org.apache.hadoop.security.LdapGroupsMapping

    An alternate implementation, which connects directly to an LDAP server to resolve the list of groups. However, this provider should only be used if the required groups reside exclusively in LDAP, and are not materialized on the Unix servers. LdapGroupsMapping supports SSL connection and POSIX group semantics. See section [LDAP Groups Mapping](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/GroupsMapping.html#LDAP\_Groups\_Mapping) for details.
*   org.apache.hadoop.security.CompositeGroupsMapping

    This implementation composites other group mapping providers for determining group membership. This allows to combine existing provider implementations and composite a virtually new provider without customized development to deal with complex situation. See section [Composite Groups Mapping](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/GroupsMapping.html#Composite\_Groups\_Mapping) for details.

For HDFS, the mapping of users to groups is performed on the NameNode. Thus, the host system configuration of the NameNode determines the group mappings for the users.

Note that HDFS stores the user and group of a file or directory as strings; there is no conversion from user and group identity numbers as is conventional in Unix.

### Static Mapping <a href="#static_mapping" id="static_mapping"></a>

It is possible to statically map users to groups by defining the mapping in `hadoop.user.group.static.mapping.overrides` in the format `user1=group1,group2;user2=;user3=group2`. This property overrides any group mapping service provider. If a user’s groups are defined in it, the groups are returned without more lookups; otherwise, the service provider defined in `hadoop.security.group.mapping` is used to look up the groups. By default, `dr.who=;` is defined, so the fake user dr.who will not have any groups.

### Caching/Negative caching <a href="#caching.2fnegative_caching" id="caching.2fnegative_caching"></a>

Since the group mapping resolution relies on external mechanisms, the NameNode performance may be impacted. To reduce the impact due to repeated lookups, Hadoop caches the groups returned by the service provider. The cache invalidate is configurable via `hadoop.security.groups.cache.secs`, and the default is 300 seconds.

With the default caching implementation, after `hadoop.security.groups.cache.secs` when the cache entry expires, the next thread to request group membership will query the group mapping service provider to lookup the current groups for the user. While this lookup is running, the thread that initiated it will block, while any other threads requesting groups for the same user will retrieve the previously cached values. If the refresh fails, the thread performing the refresh will throw an exception and the process will repeat for the next thread that requests a lookup for that value. If the lookup repeatedly fails, and the cache is not updated, after `hadoop.security.groups.cache.secs * 10` seconds the cached entry will be evicted and all threads will block until a successful reload is performed.

To avoid any threads blocking when the cached entry expires, set `hadoop.security.groups.cache.background.reload` to true. This enables a small thread pool of `hadoop.security.groups.cache.background.reload.threads` threads having 3 threads by default. With this setting, when the cache is queried for an expired entry, the expired result is returned immediately and a task is queued to refresh the cache in the background. If the background refresh fails a new refresh operation will be queued by the next request to the cache, until `hadoop.security.groups.cache.secs * 10` when the cached entry will be evicted and all threads will block for that user until a successful reload occurs.

To avoid spamming NameNode with unknown users, Hadoop employs negative caching so that if the result of the lookup is empty, return an empty group directly instead of performing more group mapping queries, The cache invalidation is configurable via `hadoop.security.groups.negative-cache.secs`. The default is 30 seconds, so if group mapping service providers returns no group for a user, no lookup will be performed for the same user within 30 seconds.

### LDAP Groups Mapping <a href="#ldap_groups_mapping" id="ldap_groups_mapping"></a>

This provider supports LDAP with simple password authentication using JNDI API. `hadoop.security.group.mapping.ldap.url` must be set. This refers to the URL of the LDAP server(s) for resolving user groups. It supports configuring multiple LDAP servers via a comma-separated list.

`hadoop.security.group.mapping.ldap.base` configures the search base for the LDAP connection. This is a distinguished name, and will typically be the root of the LDAP directory. Get groups for a given username first looks up the user and then looks up the groups for the user result. If the directory setup has different user and group search bases, use `hadoop.security.group.mapping.ldap.userbase` and `hadoop.security.group.mapping.ldap.groupbase` configs.

It is possible to set a maximum time limit when searching and awaiting a result. Set `hadoop.security.group.mapping.ldap.directory.search.timeout` to 0 if infinite wait period is desired. Default is 10,000 milliseconds (10 seconds). This is the limit for each ldap query. If `hadoop.security.group.mapping.ldap.search.group.hierarchy.levels` is set to a positive value, then the total latency will be bounded by max(Recur Depth in LDAP, `hadoop.security.group.mapping.ldap.search.group.hierarchy.levels` ) \* `hadoop.security.group.mapping.ldap.directory.search.timeout`.

`hadoop.security.group.mapping.ldap.base` configures how far to walk up the groups hierarchy when resolving groups. By default, with a limit of 0, in order to be considered a member of a group, the user must be an explicit member in LDAP. Otherwise, it will traverse the group hierarchy `hadoop.security.group.mapping.ldap.search.group.hierarchy.levels` levels up.

It is possible to have custom group search filters with different arguments using the configuration `hadoop.security.group.mapping.ldap.group.search.filter.pattern`, we can configure comma separated values here and the values configured will be fetched from the LDAP attributes and will be replaced in the group search filter in the order they appear here, say if the first entry here is uid, so uid will be fetched from the attributes and the value fetched will be used in place of {0} in the group search filter, similarly the second value configured will replace {1} and so on.

Note: If `hadoop.security.group.mapping.ldap.group.search.filter.pattern` is configured, the group search will always be done assuming this group search filter pattern irrespective of any other parameters.

#### Bind user(s) <a href="#bind_user.28s.29" id="bind_user.28s.29"></a>

If the LDAP server does not support anonymous binds, set the distinguished name of the user to bind in `hadoop.security.group.mapping.ldap.bind.user`. The path to the file containing the bind user’s password is specified in `hadoop.security.group.mapping.ldap.bind.password.file`. This file should be readable only by the Unix user running the daemons.

### Multiple bind users <a href="#multiple_bind_users" id="multiple_bind_users"></a>

If multiple bind users are required, they can be specified through `hadoop.security.group.mapping.ldap.bind.users`. These will represent the aliases of users to be used to bind as when connecting to the LDAP. Each alias will then have to have its distinguished name and password configured. This is useful if the bind user’s password has to be reset. If AuthenticationException is encountered when connecting to LDAP, LDAPGroupsMapping will switch to the next bind user information and cycle back if necessary.

For example, if: `hadoop.security.group.mapping.ldap.bind.users=alias1,alias2` , then the following configuration is valid: `hadoop.security.group.mapping.ldap.bind.users.alias1.bind.user=bindUser1` `hadoop.security.group.mapping.ldap.bind.users.alias1.bind.password.alias=bindPasswordAlias1` `hadoop.security.group.mapping.ldap.bind.users.alias2.bind.user=bindUser2` `hadoop.security.group.mapping.ldap.bind.users.alias2.bind.password.alias=bindPasswordAlias2`

#### Active Directory <a href="#active_directory" id="active_directory"></a>

The default configuration supports LDAP group name resolution with an Active Directory server.

#### POSIX Groups <a href="#posix_groups" id="posix_groups"></a>

If the LDAP server supports POSIX group semantics (RFC-2307), Hadoop can perform LDAP group resolution queries to the server by setting both `hadoop.security.group.mapping.ldap.search.filter.user` to `(&amp;(objectClass=posixAccount)(uid={0}))` and `hadoop.security.group.mapping.ldap.search.filter.group` to `(objectClass=posixGroup)`.

#### SSL <a href="#ssl" id="ssl"></a>

To secure the connection, the implementation supports LDAP over SSL (LDAPS). SSL is enable by setting `hadoop.security.group.mapping.ldap.ssl` to `true`. In addition, specify the path to the keystore file for SSL connection in `hadoop.security.group.mapping.ldap.ssl.keystore` and keystore password in `hadoop.security.group.mapping.ldap.ssl.keystore.password`, at the same time, make sure `hadoop.security.credential.clear-text-fallback` is true. Alternatively, store the keystore password in a file, and point `hadoop.security.group.mapping.ldap.ssl.keystore.password.file` to that file. For security purposes, this file should be readable only by the Unix user running the daemons, and for preventing recursive dependency, this file should be a local file. The first approach aka using `hadoop.security.group.mapping.ldap.ssl.keystore.password` is highly discouraged because it exposes the password in the configuration file.

#### Low latency group mapping resolution <a href="#low_latency_group_mapping_resolution" id="low_latency_group_mapping_resolution"></a>

Typically, Hadoop resolves a user’s group names by making two LDAP queries: the first query gets the user object, and the second query uses the user’s Distinguished Name to find the groups. For some LDAP servers, such as Active Directory, the user object returned in the first query also contains the DN of the user’s groups in its `memberOf` attribute, and the name of a group is its Relative Distinguished Name. Therefore, it is possible to infer the user’s groups from the first query without sending the second one, and it may reduce group name resolution latency incurred by the second query. If it fails to get group names, it will fall back to the typical two-query scenario and send the second query to get group names. To enable this feature, set `hadoop.security.group.mapping.ldap.search.attr.memberof` to `memberOf`, and Hadoop will resolve group names using this attribute in the user object.

If the LDAP server’s certificate is not signed by a well known certificate authority, specify the path to the truststore in `hadoop.security.group.mapping.ldap.ssl.truststore`. Similar to keystore, specify the truststore password file in `hadoop.security.group.mapping.ldap.ssl.truststore.password.file`.

### Composite Groups Mapping

`CompositeGroupsMapping` works by enumerating a list of service providers in `hadoop.security.group.mapping.providers`. It get groups from each of the providers in the list one after the other. If `hadoop.security.group.mapping.providers.combined` is `true`, merge the groups returned by all providers; otherwise, return the groups in the first successful provider. See the following section for a sample configuration.

## Hadoop Auth

Hadoop Auth is a Java library consisting of a client and a server components to enable Kerberos SPNEGO authentication for HTTP.

Hadoop Auth also supports additional authentication mechanisms on the client and the server side via 2 simple interfaces.

Additionally, it provides a partially implemented derivative of the Kerberos SPNEGO authentication to allow a “mixed” form of authentication where Kerberos SPNEGO is used by non-browsers while an alternate form of authentication (to be implemented by the user) is used for browsers.

### How Does Auth Works?

Hadoop Auth enforces authentication on protected resources, once authentiation has been established it sets a signed HTTP Cookie that contains an authentication token with the user name, user principal, authentication type and expiration time.

Subsequent HTTP client requests presenting the signed HTTP Cookie have access to the protected resources until the HTTP Cookie expires.

The secret used to sign the HTTP Cookie has multiple implementations that provide different behaviors, including a hardcoded secret string, a rolling randomly generated secret, and a rolling randomly generated secret synchronized between multiple servers using ZooKeeper.

### User Documentation <a href="#user_documentation" id="user_documentation"></a>

* [Examples](https://hadoop.apache.org/docs/current/hadoop-auth/Examples.html)
* [Configuration](https://hadoop.apache.org/docs/current/hadoop-auth/Configuration.html)
* [Building It](https://hadoop.apache.org/docs/current/hadoop-auth/BuildingIt.html)

## Transparent Encryption in HDFS

### Overview

HDFS implements _transparent_, _end-to-end_ encryption. Once configured, data read from and written to special HDFS directories is _transparently_ encrypted and decrypted without requiring changes to user application code. This encryption is also _end-to-end_, which means the data can only be encrypted and decrypted by the client. HDFS never stores or has access to unencrypted data or unencrypted data encryption keys. This satisfies two typical requirements for encryption: _at-rest encryption_ (meaning data on persistent media, such as a disk) as well as _in-transit encryption_ (e.g. when data is travelling over the network).

### Background <a href="#background" id="background"></a>

Encryption can be done at different layers in a traditional data management software/hardware stack. Choosing to encrypt at a given layer comes with different advantages and disadvantages.

* Application-level encryption. This is the most secure and most flexible approach. The application has ultimate control over what is encrypted and can precisely reflect the requirements of the user. However, writing applications to do this is hard. This is also not an option for customers of existing applications that do not support encryption.
* Database-level encryption. Similar to application-level encryption in terms of its properties. Most database vendors offer some form of encryption. However, there can be performance issues. One example is that indexes cannot be encrypted.
* Filesystem-level encryption. This option offers high performance, application transparency, and is typically easy to deploy. However, it is unable to model some application-level policies. For instance, multi-tenant applications might want to encrypt based on the end user. A database might want different encryption settings for each column stored within a single file.
* Disk-level encryption. Easy to deploy and high performance, but also quite inflexible. Only really protects against physical theft.

HDFS-level encryption fits between database-level and filesystem-level encryption in this stack. This has a lot of positive effects. HDFS encryption is able to provide good performance and existing Hadoop applications are able to run transparently on encrypted data. HDFS also has more context than traditional filesystems when it comes to making policy decisions.

HDFS-level encryption also prevents attacks at the filesystem-level and below (so-called “OS-level attacks”). The operating system and disk only interact with encrypted bytes, since the data is already encrypted by HDFS.

### Use Cases <a href="#use_cases" id="use_cases"></a>

Data encryption is required by a number of different government, financial, and regulatory entities. For example, the health-care industry has HIPAA regulations, the card payment industry has PCI DSS regulations, and the US government has FISMA regulations. Having transparent encryption built into HDFS makes it easier for organizations to comply with these regulations.

Encryption can also be performed at the application-level, but by integrating it into HDFS, existing applications can operate on encrypted data without changes. This integrated architecture implies stronger encrypted file semantics and better coordination with other HDFS functions.

### Architecture <a href="#architecture" id="architecture"></a>

#### Overview <a href="#overview" id="overview"></a>

For transparent encryption, we introduce a new abstraction to HDFS: the _encryption zone_. An encryption zone is a special directory whose contents will be transparently encrypted upon write and transparently decrypted upon read. Each encryption zone is associated with a single _encryption zone key_ which is specified when the zone is created. Each file within an encryption zone has its own unique _data encryption key (DEK)_. DEKs are never handled directly by HDFS. Instead, HDFS only ever handles an _encrypted data encryption key (EDEK)_. Clients decrypt an EDEK, and then use the subsequent DEK to read and write data. HDFS datanodes simply see a stream of encrypted bytes.

A very important use case of encryption is to “switch it on” and ensure all files across the entire filesystem are encrypted. To support this strong guarantee without losing the flexibility of using different encryption zone keys in different parts of the filesystem, HDFS allows _nested encryption zones_. After an encryption zone is created (e.g. on the root directory `/`), a user can create more encryption zones on its descendant directories (e.g. `/home/alice`) with different keys. The EDEK of a file will be generated using the encryption zone key from the closest ancestor encryption zone.

A new cluster service is required to manage encryption keys: the Hadoop Key Management Server (KMS). In the context of HDFS encryption, the KMS performs three basic responsibilities:

1. Providing access to stored encryption zone keys
2. Generating new encrypted data encryption keys for storage on the NameNode
3. Decrypting encrypted data encryption keys for use by HDFS clients

The KMS will be described in more detail below.

#### Accessing data within an encryption zone <a href="#accessing_data_within_an_encryption_zone" id="accessing_data_within_an_encryption_zone"></a>

When creating a new file in an encryption zone, the NameNode asks the KMS to generate a new EDEK encrypted with the encryption zone’s key. The EDEK is then stored persistently as part of the file’s metadata on the NameNode.

When reading a file within an encryption zone, the NameNode provides the client with the file’s EDEK and the encryption zone key version used to encrypt the EDEK. The client then asks the KMS to decrypt the EDEK, which involves checking that the client has permission to access the encryption zone key version. Assuming that is successful, the client uses the DEK to decrypt the file’s contents.

All of the above steps for the read and write path happen automatically through interactions between the DFSClient, the NameNode, and the KMS.

Access to encrypted file data and metadata is controlled by normal HDFS filesystem permissions. This means that if HDFS is compromised (for example, by gaining unauthorized access to an HDFS superuser account), a malicious user only gains access to ciphertext and encrypted keys. However, since access to encryption zone keys is controlled by a separate set of permissions on the KMS and key store, this does not pose a security threat.

#### Key Management Server, KeyProvider, EDEKs <a href="#key_management_server.2c_keyprovider.2c_edeks" id="key_management_server.2c_keyprovider.2c_edeks"></a>

The KMS is a proxy that interfaces with a backing key store on behalf of HDFS daemons and clients. Both the backing key store and the KMS implement the Hadoop KeyProvider API. See the [KMS documentation](https://hadoop.apache.org/docs/current/hadoop-kms/index.html) for more information.

In the KeyProvider API, each encryption key has a unique _key name_. Because keys can be rolled, a key can have multiple _key versions_, where each key version has its own _key material_ (the actual secret bytes used during encryption and decryption). An encryption key can be fetched by either its key name, returning the latest version of the key, or by a specific key version.

The KMS implements additional functionality which enables creation and decryption of _encrypted encryption keys (EEKs)_. Creation and decryption of EEKs happens entirely on the KMS. Importantly, the client requesting creation or decryption of an EEK never handles the EEK’s encryption key. To create a new EEK, the KMS generates a new random key, encrypts it with the specified key, and returns the EEK to the client. To decrypt an EEK, the KMS checks that the user has access to the encryption key, uses it to decrypt the EEK, and returns the decrypted encryption key.

In the context of HDFS encryption, EEKs are _encrypted data encryption keys (EDEKs)_, where a _data encryption key (DEK)_ is what is used to encrypt and decrypt file data. Typically, the key store is configured to only allow end users access to the keys used to encrypt DEKs. This means that EDEKs can be safely stored and handled by HDFS, since the HDFS user will not have access to unencrypted encryption keys.

### Configuration <a href="#configuration" id="configuration"></a>

A necessary prerequisite is an instance of the KMS, as well as a backing key store for the KMS. See the [KMS documentation](https://hadoop.apache.org/docs/current/hadoop-kms/index.html) for more information.

Once a KMS has been set up and the NameNode and HDFS clients have been correctly configured, an admin can use the `hadoop key` and `hdfs crypto` command-line tools to create encryption keys and set up new encryption zones. Existing data can be encrypted by copying it into the new encryption zones using tools like distcp.

#### Configuring the cluster KeyProvider <a href="#configuring_the_cluster_keyprovider" id="configuring_the_cluster_keyprovider"></a>

**hadoop.security.key.provider.path**

The KeyProvider to use when interacting with encryption keys used when reading and writing to an encryption zone. HDFS clients will use the provider path returned from Namenode via getServerDefaults. If namenode doesn’t support returning key provider uri then client’s conf will be used.

#### Selecting an encryption algorithm and codec <a href="#selecting_an_encryption_algorithm_and_codec" id="selecting_an_encryption_algorithm_and_codec"></a>

**hadoop.security.crypto.codec.classes.EXAMPLECIPHERSUITE**

The prefix for a given crypto codec, contains a comma-separated list of implementation classes for a given crypto codec (eg EXAMPLECIPHERSUITE). The first implementation will be used if available, others are fallbacks.

**hadoop.security.crypto.codec.classes.aes.ctr.nopadding**

Default: `org.apache.hadoop.crypto.OpensslAesCtrCryptoCodec, org.apache.hadoop.crypto.JceAesCtrCryptoCodec`

Comma-separated list of crypto codec implementations for AES/CTR/NoPadding. The first implementation will be used if available, others are fallbacks.

**hadoop.security.crypto.codec.classes.sm4.ctr.nopadding**

Default: `org.apache.hadoop.crypto.OpensslSm4CtrCryptoCodec, org.apache.hadoop.crypto.JceSm4CtrCryptoCodec`

Comma-separated list of crypto codec implementations for SM4/CTR/NoPadding. The first implementation will be used if available, others are fallbacks.

**hadoop.security.crypto.cipher.suite**

Default: `AES/CTR/NoPadding`

Cipher suite for crypto codec, now AES/CTR/NoPadding and SM4/CTR/NoPadding are supported.

**hadoop.security.crypto.jce.provider**

Default: None

The JCE provider name used in CryptoCodec.

**hadoop.security.crypto.buffer.size**

Default: `8192`

The buffer size used by CryptoInputStream and CryptoOutputStream.

#### Namenode configuration <a href="#namenode_configuration" id="namenode_configuration"></a>

**dfs.namenode.list.encryption.zones.num.responses**

Default: `100`

When listing encryption zones, the maximum number of zones that will be returned in a batch. Fetching the list incrementally in batches improves namenode performance.

### `crypto` command-line interface <a href="#crypto_command-line_interface" id="crypto_command-line_interface"></a>

#### createZone <a href="#createzone" id="createzone"></a>

Usage: `[-createZone -keyName <keyName> -path <path>]`

Create a new encryption zone.

|           |                                                                                                                             |
| --------- | --------------------------------------------------------------------------------------------------------------------------- |
| _path_    | The path of the encryption zone to create. It must be an empty directory. A trash directory is provisioned under this path. |
| _keyName_ | Name of the key to use for the encryption zone. Uppercase key names are unsupported.                                        |

#### listZones <a href="#listzones" id="listzones"></a>

Usage: `[-listZones]`

List all encryption zones. Requires superuser permissions.

#### provisionTrash <a href="#provisiontrash" id="provisiontrash"></a>

Usage: `[-provisionTrash -path <path>]`

Provision a trash directory for an encryption zone.

|        |                                              |
| ------ | -------------------------------------------- |
| _path_ | The path to the root of the encryption zone. |

#### getFileEncryptionInfo <a href="#getfileencryptioninfo" id="getfileencryptioninfo"></a>

Usage: `[-getFileEncryptionInfo -path <path>]`

Get encryption information from a file. This can be used to find out whether a file is being encrypted, and the key name / key version used to encrypt it.

|        |                                                     |
| ------ | --------------------------------------------------- |
| _path_ | The path of the file to get encryption information. |

#### reencryptZone <a href="#reencryptzone" id="reencryptzone"></a>

Usage: `[-reencryptZone <action> -path <zone>]`

Re-encrypts an encryption zone, by iterating through the encryption zone, and calling the KeyProvider’s reencryptEncryptedKeys interface to batch-re-encrypt all files’ EDEKs with the latest version encryption zone key in the key provider. Requires superuser permissions.

Note that re-encryption does not apply to snapshots, due to snapshots’ immutable nature.

|          |                                                                         |
| -------- | ----------------------------------------------------------------------- |
| _action_ | The re-encrypt action to perform. Must be either `-start` or `-cancel`. |
| _path_   | The path to the root of the encryption zone.                            |

Re-encryption is a NameNode-only operation in HDFS, so could potentially put intensive load to the NameNode. The following configurations can be changed to control the stress on the NameNode, depending on the acceptable throughput impact to the cluster.

|                                                       |                                                                                                                                                                                                             |
| ----------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| _dfs.namenode.reencrypt.batch.size_                   | The number of EDEKs in a batch to be sent to the KMS for re-encryption. Each batch is processed when holding the name system read/write lock, with throttling happening between batches. See configs below. |
| _dfs.namenode.reencrypt.throttle.limit.handler.ratio_ | Ratio of read locks to be held during re-encryption. 1.0 means no throttling. 0.5 means re-encryption can hold the readlock at most 50% of its total processing time. Negative value or 0 are invalid.      |
| _dfs.namenode.reencrypt.throttle.limit.updater.ratio_ | Ratio of write locks to be held during re-encryption. 1.0 means no throttling. 0.5 means re-encryption can hold the writelock at most 50% of its total processing time. Negative value or 0 are invalid.    |

#### listReencryptionStatus <a href="#listreencryptionstatus" id="listreencryptionstatus"></a>

Usage: `[-listReencryptionStatus]`

List re-encryption information for all encryption zones. Requires superuser permissions.

### Distcp considerations

#### Running as the superuser <a href="#running_as_the_superuser" id="running_as_the_superuser"></a>

One common usecase for distcp is to replicate data between clusters for backup and disaster recovery purposes. This is typically performed by the cluster administrator, who is an HDFS superuser.

To enable this same workflow when using HDFS encryption, we introduced a new virtual path prefix, `/.reserved/raw/`, that gives superusers direct access to the underlying block data in the filesystem. This allows superusers to distcp data without needing having access to encryption keys, and also avoids the overhead of decrypting and re-encrypting data. It also means the source and destination data will be byte-for-byte identical, which would not be true if the data was being re-encrypted with a new EDEK.

When using `/.reserved/raw` to distcp encrypted data, it’s important to preserve extended attributes with the [-px](https://hadoop.apache.org/docs/current/hadoop-distcp/DistCp.html#Command\_Line\_Options) flag. This is because encrypted file attributes (such as the EDEK) are exposed through extended attributes within `/.reserved/raw`, and must be preserved to be able to decrypt the file. This means that if the distcp is initiated at or above the encryption zone root, it will automatically create an encryption zone at the destination if it does not already exist. However, it’s still recommended that the admin first create identical encryption zones on the destination cluster to avoid any potential mishaps.

#### Copying into encrypted locations <a href="#copying_into_encrypted_locations" id="copying_into_encrypted_locations"></a>

By default, distcp compares checksums provided by the filesystem to verify that the data was successfully copied to the destination. When copying from unencrypted or encrypted location into an encrypted location, the filesystem checksums will not match since the underlying block data is different because a new EDEK will be used to encrypt at destination. In this case, specify the [-skipcrccheck](https://hadoop.apache.org/docs/current/hadoop-distcp/DistCp.html#Command\_Line\_Options) and [-update](https://hadoop.apache.org/docs/current/hadoop-distcp/DistCp.html#Command\_Line\_Options) distcp flags to avoid verifying checksums.

### Rename and Trash considerations <a href="#rename_and_trash_considerations" id="rename_and_trash_considerations"></a>

HDFS restricts file and directory renames across encryption zone boundaries. This includes renaming an encrypted file / directory into an unencrypted directory (e.g., `hdfs dfs mv /zone/encryptedFile /home/bob`), renaming an unencrypted file or directory into an encryption zone (e.g., `hdfs dfs mv /home/bob/unEncryptedFile /zone`), and renaming between two different encryption zones (e.g., `hdfs dfs mv /home/alice/zone1/foo /home/alice/zone2`). In these examples, `/zone`, `/home/alice/zone1`, and `/home/alice/zone2` are encryption zones, while `/home/bob` is not. A rename is only allowed if the source and destination paths are in the same encryption zone, or both paths are unencrypted (not in any encryption zone).

This restriction enhances security and eases system management significantly. All file EDEKs under an encryption zone are encrypted with the encryption zone key. Therefore, if the encryption zone key is compromised, it is important to identify all vulnerable files and re-encrypt them. This is fundamentally difficult if a file initially created in an encryption zone can be renamed to an arbitrary location in the filesystem.

To comply with the above rule, each encryption zone has its own `.Trash` directory under the “zone directory”. E.g., after `hdfs dfs rm /zone/encryptedFile`, `encryptedFile` will be moved to `/zone/.Trash`, instead of the `.Trash` directory under the user’s home directory. When the entire encryption zone is deleted, the “zone directory” will be moved to the `.Trash` directory under the user’s home directory.

If the encryption zone is the root directory (e.g., `/` directory), the trash path of root directory is `/.Trash`, not the `.Trash` directory under the user’s home directory, and the behavior of renaming sub-directories or sub-files in root directory will keep consistent with the behavior in a general encryption zone, such as `/zone` which is mentioned at the top of this section.

The `crypto` command before Hadoop 2.8.0 does not provision the `.Trash` directory automatically. If an encryption zone is created before Hadoop 2.8.0, and then the cluster is upgraded to Hadoop 2.8.0 or above, the trash directory can be provisioned using `-provisionTrash` option (e.g., `hdfs crypto -provisionTrash -path /zone`).

### Attack vectors <a href="#attack_vectors" id="attack_vectors"></a>

#### Hardware access exploits <a href="#hardware_access_exploits" id="hardware_access_exploits"></a>

These exploits assume that attacker has gained physical access to hard drives from cluster machines, i.e. datanodes and namenodes.

1. Access to swap files of processes containing data encryption keys.
   * By itself, this does not expose cleartext, as it also requires access to encrypted block files.
   * This can be mitigated by disabling swap, using encrypted swap, or using mlock to prevent keys from being swapped out.
2. Access to encrypted block files.
   * By itself, this does not expose cleartext, as it also requires access to DEKs.

#### Root access exploits <a href="#root_access_exploits" id="root_access_exploits"></a>

These exploits assume that attacker has gained root shell access to cluster machines, i.e. datanodes and namenodes. Many of these exploits cannot be addressed in HDFS, since a malicious root user has access to the in-memory state of processes holding encryption keys and cleartext. For these exploits, the only mitigation technique is carefully restricting and monitoring root shell access.

1. Access to encrypted block files.
   * By itself, this does not expose cleartext, as it also requires access to encryption keys.
2. Dump memory of client processes to obtain DEKs, delegation tokens, cleartext.
   * No mitigation.
3. Recording network traffic to sniff encryption keys and encrypted data in transit.
   * By itself, insufficient to read cleartext without the EDEK encryption key.
4. Dump memory of datanode process to obtain encrypted block data.
   * By itself, insufficient to read cleartext without the DEK.
5. Dump memory of namenode process to obtain encrypted data encryption keys.
   * By itself, insufficient to read cleartext without the EDEK’s encryption key and encrypted block files.

#### HDFS admin exploits <a href="#hdfs_admin_exploits" id="hdfs_admin_exploits"></a>

These exploits assume that the attacker has compromised HDFS, but does not have root or `hdfs` user shell access.

1. Access to encrypted block files.
   * By itself, insufficient to read cleartext without the EDEK and EDEK encryption key.
2. Access to encryption zone and encrypted file metadata (including encrypted data encryption keys), via -fetchImage.
   * By itself, insufficient to read cleartext without EDEK encryption keys.

#### Rogue user exploits <a href="#rogue_user_exploits" id="rogue_user_exploits"></a>

A rogue user can collect keys of files they have access to, and use them later to decrypt the encrypted data of those files. As the user had access to those files, they already had access to the file contents. This can be mitigated through periodic key rolling policies. The [reencryptZone](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/TransparentEncryption.html#reencryptZone) command is usually required after key rolling, to make sure the EDEKs on existing files use the new version key.

Manual steps to a complete key rolling and re-encryption are listed below. These instructions assume that you are running as the key admin or HDFS superuser as is appropriate.
