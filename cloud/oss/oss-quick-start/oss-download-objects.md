# Download objects

This topic describes how to download one or more objects from a bucket in the OSS console.

### Prerequisites

Objects have been uploaded to the bucket. For more information, see [Upload Objects](oss-upload-objects.md).

### Use the OSS console

1. Log on to the [Cloud Console](http://console.cloud.bigconnect.io/).
2. In the left-side navigation pane, click **Object Storage**.
3. Click the target bucket.
4. Click on one or more files to select them.
5. Click the **Download object** button or **Download as zip** if multiple files are selected

### Use osscli

**`osscli`** is a command line tool for OSS. You can use **`osscli cp`**command to download files from a bucket.

```bash
osscli cp bdl/mybucket/myobject.txt /tmp
```

### Use APIs and SDKs

OSS provides APIs and SDKs that use a variety of programming languages to facilitate secondary development.



