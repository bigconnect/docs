# Configuring File-Based Authentication

### Configuring File-Based Authentication <a href="#ariaid-title10" id="ariaid-title10"></a>

If your organization does not use LDAP and you want to enable multiple users to access BigConnect Pipelines, you might configure BigConnect Pipelines to use file-based authentication.

To configure file-based authentication, perform the following tasks:

1. Configure authentication properties.
2. Configure BigConnect Pipelines users, groups, and roles.

Users can change their password after logging into BigConnect Pipelines.

#### Step 1. Configure Authentication Properties <a href="#ariaid-title11" id="ariaid-title11"></a>

Configure authentication properties in the BigConnect Pipelines configuration file, `$SDC_CONF/sdc.properties`.

When you use file-based authentication, you can use the Basic, Digest, or Form authentication type.

1. In the BigConnect Pipelines configuration file, `$SDC_CONF/sdc.properties`, enable file-based authentication by setting the **http.authentication.login.module** property to **file**.
2. In the `$SDC_CONF/sdc.properties` file, define the HTTP authentication type by setting the **http.authentication** property to **basic**, **digest**, or **form**.
3.  Specify whether BigConnect Pipelines checks the permissions for the associated realm.properties file for the type of authentication that you use. Set the **http.realm.file.permission.check** property to one of the following values:

    * **true** to ensure that the realm.properties file allows access only to the owner.
    * **false** to skip the permission check.

    You'll use the realm.properties file in the next step, when you configure BigConnect Pipelines users and roles.

#### Step 2. Configure Users, Groups, and Roles <a href="#ariaid-title12" id="ariaid-title12"></a>

For file-based authentication, you configure the users that can log in to BigConnect Pipelines. You assign roles to each user account and you can optionally create and assign groups to the user accounts.

Configure users, groups, and roles in the properties file for the type of authentication that you use: `$SDC_CONF/<authentication>-realm.properties`.

BigConnect Pipelines roles determine the tasks that a user can perform. You can also create groups and assign the groups to related user accounts. Use groups to easily assign pipeline permissions to groups of users. Pipeline permissions determine the pipeline access that each user has.

For example, say you use file-based authentication and want to create an `Ops` group to manage pipelines. To handle this, when you configure users in the authentication properties file, you grant the Manager role and add the `Ops` group for each operations user. Then, you edit each pipeline they need to manage, assigning read and execute permission to the `Ops` group. For more information, see Roles and Permissions.

BigConnect Pipelines provides several default user accounts and groups. You can change or remove these default user accounts and groups. For increased security, change the passwords for the default user accounts.\
\
**Note**: BigConnect Pipelines installed through a cloud service provider marketplace includes only the default `admin` user account and no default groups.\
\
For file-based authentication, BigConnect Pipelines provides the following default user accounts with corresponding roles:

| User Login            | Role    | Tasks                                                                                                                                                                                                                                                                                          |
| --------------------- | ------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `admin` / `admin`     | Admin   | Perform any BigConnect Pipelines task. Can perform all tasks listed below, as well as activate BigConnect Pipelines, restart and shut down BigConnect Pipelines, and view BigConnect Pipelines metrics. Enable Control Hub. Install libraries using Package Manager. Generate support bundles. |
| `manager` / `manager` | Manager | Start and stop pipelines, monitor pipelines, configure and reset alerts. Take, review, and manage snapshots.                                                                                                                                                                                   |
| `creator` / `creator` | Creator | Create and configure pipelines and alerts, preview data, and monitor the pipeline. Import pipelines.                                                                                                                                                                                           |
| `guest` / `guest`     | Guest   | View pipelines and alerts, and general monitoring information. Export a pipeline.                                                                                                                                                                                                              |

For file-based authentication, BigConnect Pipelines also provides a default `all` group that includes every user, and a `dev` and `test` group. The following default user accounts are available for the `dev` and `test` user groups:

| User Login        | Roles               | Group |
| ----------------- | ------------------- | ----- |
| `user1` / `user1` | Manager and Creator | dev   |
| `user2` / `user2` | Manager and Creator | dev   |
| `user3` / `user3` | Manager and Creator | test  |
| `user4` / `user4` | Manager and Creator | test  |

Configure users and groups in the properties file for the type of authentication that you use. For example, if you use basic authentication, use the `basic-realm.properties` file.

To hash login information, you can use an md5 program such as md5 on Mac OS X or md5sum on Linux. For example, you might use the following command to hash a password so that the password is not displayed in the prompt:

```
read -s pw && echo "$pw" | md5
```

For basic and form authentication, hash the password alone. For example, when the above command prompts you for the password, enter only the password.

For digest authentication, hash the combination of `<user name>:<realm>:<password>`, where `<realm>` is the authentication type. For example, when the above command prompts you for the password, enter:

`<user name>:<realm>:<password>`

as follows:

`jdoe:digest-realm:JdoePass`

1. To configure users and groups, modify the properties file for the type of authentication that you use.\
   The file name is $SDC\_CONF/\<authentication>-realm.properties.
2.  For each new user, add a user definition using the following format:

    ```
    <user name>: MD5:<md5-text>, user, <role> [, <additional role>, <additional role>...] [, group:<group>, group:<additional group>...]
    ```

    \
    **Note**: Assign one or more roles to each user. Be sure to include `user` in every user definition.\
    \
    For example, the following line defines a user named jsmith assigned the Creator role and to the Development group:

    ```
    jsmith: MD5:6d0258c2440a7d19e916292b231e3190,user,creator,group:Development
    ```
3. To make the new users available, restart BigConnect Pipelines.

#### Changing Your Password <a href="#ariaid-title13" id="ariaid-title13"></a>

When BigConnect Pipelines is configured for file-based authentication, you can use the BigConnect Pipelines UI to change your password.

**Note**: When BigConnect Pipelines is installed through Cloudera Manager, you cannot use the BigConnect Pipelines UI to change your password. BigConnect Pipelines configuration properties, including file-based authentication, are administered through Cloudera Manager.

1. Click the **User** icon (![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/icon\_OverUser.png)), and then click **Change Password**.
2.  Enter your current and new password, verify the new password, and then click **Save**.

    Your changed password takes effect the next time you log in to BigConnect Pipelines.
