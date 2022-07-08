# Create Buckets

After you activate the OSS service, you must create a bucket in the OSS console to store objects.

## Use the OSS console

To create a bucket in the OSS console, perform the following steps:

1. Log on to the [Cloud Console](http://console.cloud.bigconnect.io/).
2. In the left-side navigation pane, click **Object Storage**. On the page that appears, click **Create Bucket**.
3.  In the **Create Bucket** dialog box that appears, enter the name of the bucket.\


    The name cannot be changed after the bucket is created. The naming conventions are as follows:

    * The bucket name must be globally unique in a Data Lake OSS.
    * The name can contain only lowercase letters, digits, and hyphens (-).
    * The name must start and end with a lowercase letter or digit.
    * The name must be 3 to 63 bytes in length.\

4. Click **OK**.

## Use osscli

**`osscli`** is a command line tool for OSS. You can use **`osscli mb`** to create a bucket.

```
osscli mb bdl/mybucket
```

## Use APIs and SDKs

OSS provides APIs and SDKs that use a variety of programming languages to facilitate secondary development.





