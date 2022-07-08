# Upload Objects

After a bucket is created, you can upload any type of object to it.

### Prerequisites

A bucket has been created. For more information, see [Create a bucket](oss-create-buckets.md).

### Use the OSS console

To upload an object to an OSS bucket in the OSS console, perform the following steps:

1. Log on to the [Cloud Console](http://console.cloud.bigconnect.io/).
2. In the left-side navigation pane, click **Object Storage**.
3. Click the target bucket.
4. Click the <img src="../../../.gitbook/assets/image (642).png" alt="" data-size="original"> icon in the lower part of the screen and choose the **Upload file** icon
5. Choose one or more files to upload.

{% hint style="success" %}
You can also drag and drop files onto the to upload them.
{% endhint %}

### Use osscli

**`osscli`** is a command line tool for OSS. You can use **`osscli cp`**command to upload files to a bucket.

```bash
osscli cp myobject.txt bdl/mybucket
```

### Use APIs and SDKs

OSS provides APIs and SDKs that use a variety of programming languages to facilitate secondary development.
