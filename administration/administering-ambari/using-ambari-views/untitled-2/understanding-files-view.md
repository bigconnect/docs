# Understanding Files View

You can access files and folders in a cluster's file system using the Ambari web user interface.

Ambari automatically creates one Files View, pointed to the HDFS file system, during cluster deployment. You can also create a custom view from the Files View instance that points to an Amazon Web Services \(AWS\) S3 file system bucket.

To create a custom Files View to work with files in S3, provide the bucket URL, and valid AWS credentials in the **Settings &gt; View Configs** field when you create the view instance that accesses an S3 bucket. You must enter the AWS credentials as a single string containing no space characters using the following syntax:

**fs.defaultFS=s3a://**\[BUCKET\_URL\]**/;** **fs.s3a.access.key=**\[VALID\_ACCESS\_KEY\_STRING\]**;** **fs.s3a.secret.key**\[VALID\_SECRET\_KEY\_STRING\]

