# Delete Buckets

If you no longer use a bucket, delete it to avoid incurring further fees.

### Prerequisites

To delete a bucket, ensure that all objects in the bucket are deleted, including parts generated from incomplete multipart upload tasks. Otherwise, the bucket cannot be deleted.

{% hint style="danger" %}
Deleted objects cannot be recovered. We recommended that you proceed with caution when you perform this operation.
{% endhint %}

### Use the OSS console

1. Log on to the [Cloud Console](http://console.cloud.bigconnect.io/).
2. In the left-side navigation pane, click **Object Storage**.
3. Click the three dots next to the bucket name in the left sidebar.
4. Click **Delete.**

### Use osscli

**`osscli`** is a command line tool for OSS. You can use **`osscli rb`**command to delete files from a bucket.

```bash
osscli rb bdl/mybucket
```

### Use APIs and SDKs

OSS provides APIs and SDKs that use a variety of programming languages to facilitate secondary development.

