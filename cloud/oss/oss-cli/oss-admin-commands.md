# Admin Commands

### config

`config host` command provides a convenient way to manage host entries in your config file `~/.osscli/config.json`. It is also OK to edit the config file manually using a text editor.

```text
USAGE:
  osscli config host COMMAND [COMMAND FLAGS | -h] [ARGUMENTS...]

COMMANDS:
  add, a      add a new host to configuration file
  remove, rm  remove a host from configuration file
  list, ls    lists hosts in configuration file

FLAGS:
  --help, -h                       show help
```

_Example: Manage Config File_

Add OSS server access and secret keys to config file host entry. Note that, the history feature of your shell may record these keys and pose a security risk. On `bash` shell, use `set -o` and `set +o` to disable and enable history feature momentarily.

```text
set +o history
osscli config host add play https://storage-170393591705357.cloud.bigconnect.io OMQAGGOL63D7UNVQFY8X GcY5RHNmnEWvD/1QxD3spEIGj+Vt9L7eHaAaBTkJ
set -o history
```

Remove the host from the config file.

```text
osscli config host remove play
```

List all configured host

```text
osscli config host list
```

### stat

`stat` command displays information on objects \(with optional prefix\) contained in the specified bucket on an object storage. On a filesystem, it behaves like `stat` command.

```text
USAGE:
   osscli stat [FLAGS] TARGET

FLAGS:
  --recursive, -r               stat all objects recursively
  --encrypt-key value           encrypt/decrypt objects (using server-side encryption with customer provided keys)
  --help, -h                    show help

ENVIRONMENT VARIABLES:
   OSS_ENCRYPT_KEY:  list of comma delimited prefix=secret values
```

_Example: Display information on a bucket named "mybucket"_ 

```text
osscli stat play/mybucket
Name      : mybucket/
Date      : 2018-02-06 18:06:51 PST
Size      : 0B
Type      : folder
```

_Example: Display information on an encrypted object "myobject" in "mybucket"_ 

```text
osscli stat play/mybucket/myobject --encrypt-key "play/mybucket=32byteslongsecretkeymustbegiven1"
Name      : myobject
Date      : 2018-03-02 11:47:13 PST
Size      : 132B
ETag      : d03ba22cd78282b7aef705bf31b8cded
Type      : file
Metadata  :
  Content-Type                                   : application/octet-stream
  X-Amz-Server-Side-Encryption-Customer-Key-Md5  : 4xSRdYsabg+s2nlsHKhgnw==
  X-Amz-Server-Side-Encryption-Customer-Algorithm: AES256
```

_Example: Display information on objects contained in the bucket named "mybucket"_ 

```text
osscli stat -r play/mybucket
Name      : mybucket/META/textfile
Date      : 2018-02-06 18:17:38 PST
Size      : 1024B
ETag      : d41d8cd98f00b204e9800998ecf8427e
Type      : file
Metadata  :
  Content-Type: application/octet-stream

Name      : mybucket/emptyfile
Date      : 2018-02-06 18:16:14 PST
Size      : 100B
ETag      : d41d8cd98f00b204e9800998ecf8427e
Type      : file
Metadata  :
  Content-Type: application/octet-stream
```

### user

`user` command to add, remove, enable, disable, list users on OSS server.

```text
NAME:
  osscli admin user - manage users

FLAGS:
  --help, -h                       show help

COMMANDS:
  add      add new user
  disable  disable user
  enable   enable user
  remove   remove user
  list     list all users
  info     display info of a user
```

_Example: Add a new user 'newuser' on OSS._

```text
osscli admin user add play/ newuser newuser123
```

_Example: Add a new user 'newuser' on OSS, using standard input._

```text
osscli admin user add play/
Enter Access Key: newuser
Enter Secret Key: newuser123
```

_Example: Disable a user 'newuser' on OSS._

```text
osscli admin user disable play/ newuser
```

_Example: Enable a user 'newuser' on OSS._

```text
osscli admin user enable play/ newuser
```

_Example: Remove user 'newuser' on OSS._

```text
osscli admin user remove play/ newuser
```

_Example: List all users on OSS._

```text
osscli admin user list --json play/
{"status":"success","accessKey":"newuser","userStatus":"enabled"}
```

_Example: Display info of a user_

```text
osscli admin user info play someuser
```

### group

`group` command to add, remove, info, list, enable, disable groups on OSS server.

```text
NAME:
  osscli admin group - manage groups

USAGE:
  osscli admin group COMMAND [COMMAND FLAGS | -h] [ARGUMENTS...]

COMMANDS:
  add      add users to a new or existing group
  remove   remove group or members from a group
  info     display group info
  list     display list of groups
  enable   Enable a group
  disable  Disable a group
```

_Example: Add a pair of users to a group 'somegroup' on OSS._

Group is created if it does not exist.

```text
osscli admin group add play somegroup someuser1 someuser2
```

_Example: Remove a pair of users from a group 'somegroup' on OSS._

```text
osscli admin group remove play somegroup someuser1 someuser2
```

_Example: Remove a group 'somegroup' on OSS._

Only works if the given group is empty.

```text
osscli admin group remove play somegroup
```

_Example: Get info on a group 'somegroup' on OSS._

```text
osscli admin group info play somegroup
```

_Example: List all groups on OSS._

```text
osscli admin group list play
```

_Example: Enable a group 'somegroup' on OSS._

```text
osscli admin group enable play somegroup
```

_Example: Disable a group 'somegroup' on OSS._

```text
osscli admin group disable play somegroup
```

### quota

`quota` command to set or get bucket quota on OSS server.

```text
NAME:
  osscli admin bucket quota - manage bucket quota

USAGE:
  osscli admin bucket quota TARGET [--fifo QUOTA | --hard QUOTA | --clear]

QUOTA
  quota accepts human-readable case-insensitive number
  suffixes such as "k", "m", "g" and "t" referring to the metric units KB,
  MB, GB and TB respectively. Adding an "i" to these prefixes, uses the IEC
  units, so that "gi" refers to "gibibyte" or "GiB". A "b" at the end is
  also accepted. Without suffixes the unit is bytes.

```

_Example: List bucket quota on bucket 'mybucket' on OSS._

```text
osscli admin bucket quota play/mybucket
```

_Example: Set a hard bucket quota of 64Mb for bucket 'mybucket' on OSS._

```text
osscli admin bucket quota play/mybucket --hard 64MB
```

_Example: Reset bucket quota configured for bucket 'mybucket' on OSS._

```text
osscli admin bucket quota play/mybucket --clear
```



