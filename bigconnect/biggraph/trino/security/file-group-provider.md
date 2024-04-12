# File group provider

Trino can map user names onto groups for easier access control and resource group management. Group file resolves group membership using a file on the coordinator.

### Group file configuration[#](https://trino.io/docs/current/security/group-file.html#group-file-configuration) <a href="#group-file-configuration" id="group-file-configuration"></a>

Enable group file by creating an `etc/group-provider.properties` file on the coordinator:

```
group-provider.name=file
file.group-file=/path/to/group.txt
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

The following configuration properties are available:

| Property              | Description                                           |
| --------------------- | ----------------------------------------------------- |
| `file.group-file`     | Path of the group file.                               |
| `file.refresh-period` | How often to reload the group file. Defaults to `5s`. |

### Group files[#](https://trino.io/docs/current/security/group-file.html#group-files) <a href="#group-files" id="group-files"></a>

#### File format[#](https://trino.io/docs/current/security/group-file.html#file-format) <a href="#file-format" id="file-format"></a>

The group file contains a list of groups and members, one per line, separated by a colon. Users are separated by a comma.

```
group_name:user_1,user_2,user_3
```
