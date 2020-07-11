# osscli

**osscli** allows you to use command lines to manage OSS data. **osscli** provides a variety of simple commands for you to manage buckets and objects. 

The following operating systems support **osscli**: Windows, Linux, and macOS.

### Install osscli

For more information about how to download and install **osscli**, see [Download and installation](osscli-download-and-installation.md).

### Configure osscli

To use osscli, you first need to tell it how to access your OSS instance using the config command:

`osscli config host add <ALIAS> <YOUR-OSS-ENDPOINT> [YOUR-ACCESS-KEY] [YOUR-SECRET-KEY]`

* `<ALIAS>` a short name to your cloud storage service
* `<YOUR-OSS-ENDPOINT>` the OSS cloud URL like `https://storage-4175974314028753.cloud.bigconnect.io`
* `[YOUR-ACCESS-KEY], [YOUR-SECRET-KEY]` an IDM username and password

Example:

```text
osscli config host add bdl https://storage-4175974314028753.cloud.bigconnect.io 4175974314028753 J7IJHaasd87MNKJHjaQ
```

### Global Options

#### Option \[--debug\]

Debug option enables debug output to console.

Example: Display verbose debug output for ls command.

```text
osscli --debug ls bdl
osscli: <DEBUG> GET / HTTP/1.1
Host: storage-170393591705357.cloud.bigconnect.io
Authorization: AWS4-HMAC-SHA256 Credential=**REDACTED**/20200710/us-east-1/s3/aws4_request, SignedHeaders=host;x-amz-content-sha256;x-amz-date, Signature=**REDACTED**
X-Amz-Content-Sha256: e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855
X-Amz-Date: 20200710T054217Z
Accept-Encoding: gzip

osscli: <DEBUG> HTTP/1.1 200 OK
Content-Length: 358
Accept-Ranges: bytes
Content-Security-Policy: block-all-mixed-content
Content-Type: application/xml
Date: Fri, 10 Jul 2020 05:42:17 GMT
Server: MinIO/DEVELOPMENT.GOGET
Vary: Origin
X-Amz-Request-Id: 16204DBBF3FCB661
X-Xss-Protection: 1; mode=block

osscli: <DEBUG> Response Time:  205.540127ms

osscli: <DEBUG> GET / HTTP/1.1
Host: storage-170393591705357.cloud.bigconnect.io
Authorization: AWS4-HMAC-SHA256 Credential=**REDACTED**/20200710/us-east-1/s3/aws4_request, SignedHeaders=host;x-amz-content-sha256;x-amz-date, Signature=**REDACTED**
X-Amz-Content-Sha256: e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855
X-Amz-Date: 20200710T054217Z
Accept-Encoding: gzip

osscli: <DEBUG> HTTP/1.1 200 OK
Content-Length: 358
Accept-Ranges: bytes
Content-Security-Policy: block-all-mixed-content
Content-Type: application/xml
Date: Fri, 10 Jul 2020 05:42:17 GMT
Server: MinIO/DEVELOPMENT.GOGET
Vary: Origin
X-Amz-Request-Id: 16204DBBF6A3E8F4
X-Xss-Protection: 1; mode=block

osscli: <DEBUG> Response Time:  43.072518ms

[...]

[2016-04-08 03:56:14 IST]     0B albums/
[2016-04-04 16:11:45 IST]     0B backup/
[2016-04-01 20:10:53 IST]     0B deebucket/
[2016-03-28 21:53:49 IST]     0B guestbucket/
```

#### Option \[--json\]

JSON option enables parseable output in [JSON lines](http://jsonlines.org/) format.

Example:

```text
osscli --json ls bdl
{"status":"success","type":"folder","lastModified":"2016-04-08T03:56:14.577+05:30","size":0,"key":"albums/"}
{"status":"success","type":"folder","lastModified":"2016-04-04T16:11:45.349+05:30","size":0,"key":"backup/"}
{"status":"success","type":"folder","lastModified":"2016-04-01T20:10:53.941+05:30","size":0,"key":"deebucket/"}
{"status":"success","type":"folder","lastModified":"2016-03-28T21:53:49.217+05:30","size":0,"key":"guestbucket/"}
```

#### Option \[--no-color\]

This option disables the color theme. It is useful for dumb terminals.

#### Option \[--quiet\]

Quiet option suppress chatty console output.

#### Option \[--config-dir\]

Use this option to set a custom config path.

### FS commands

The following commands are supported by **ossutil**:

| Command | Description |
| :--- | :--- |
| ls | list buckets and objects |
| cp | copy objects |
| share | generate URL for temporary access to an object |
| diff | list differences in object name, size, and date between two buckets |
| head | display first 'n' lines of an object |
| mv | move objects |
| tree | list buckets and objects in a tree format |
| rb | remove a bucket |
| rm |  remove objects |
| mirror | synchronize object\(s\) to a remote site |
| policy | set public policy on bucket or prefix |
| watch | watch for events |
| lock | set and get object lock configuration |
| sql | run sql queries on objects |
| mb |  make a bucket |
| pipe | stream STDIN to an object |
| find | find files and objects |
| event | manage events on your buckets |
| retention | set retention for object\(s\) |
| legalhold | set legal hold for object\(s\) |
| cat | display object contents |

### Admin commands

| Command | Description |
| :--- | :--- |
| user | manage users |
| group | manage groups |
| policy | manage policies defined in the server |
| config | manage config file |
| stat | stat contents of objects and folders |
| quota | manage bucket quota |

