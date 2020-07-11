# Delete Objects

If you no longer need objects in a bucket, delete them from the bucket to avoid incurring further fees.

{% hint style="danger" %}
**Warning** You cannot recover deleted objects. Exercise caution when you perform this operation.
{% endhint %}

### Use the OSS console

1. Log on to the [Cloud Console](http://console.cloud.bigconnect.io/).
2. In the left-side navigation pane, click **Object Storage**.
3. Click the target bucket.
4. Click on one or more files to select them.
5. Click the **Delete selected** button

### Use osscli

**`osscli`** is a command line tool for OSS. You can use **`osscli rm`**command to delete files from a bucket.

```bash
osscli rm bdl/mybucket/myobject.txt
```

### Use APIs and SDKs

OSS provides APIs and SDKs that use a variety of programming languages to facilitate secondary development.



