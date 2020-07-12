# Java SDK Reference

### Create OSS Client.

```java
MinioClient minioClient =
    MinioClient.builder()
        .endpoint("https://storage-4175974314028753.cloud.bigconnect.io")
        .credentials("Q3AM3UQ867SPQQA43P2F", "zuf+tfteSlswRu7BJ86wekitnifILbZam1KYY3TG")
        .build();
```

| Bucket operations | Object operations |
| :--- | :--- |
| `bucketExists` | `composeObject` |
| `deleteBucketEncryption` | `copyObject` |
| `deleteBucketLifeCycle` | `deleteObjectTags` |
| `deleteBucketNotification` | `disableObjectLegalHold` |
| `deleteBucketPolicy` | `downloadObject` |
| `deleteBucketTags` | `enableObjectLegalHold` |
| `deleteDefaultRetention` | `getObject` |
| `disableVersioning` | `getObjectRetention` |
| `enableVersioning` | `getObjectTags` |
| `getBucketEncryption` | `getObjectUrl` |
| `getBucketLifeCycle` | `getPresignedObjectUrl` |
| `getBucketNotification` | `isObjectLegalHoldEnabled` |
| `getBucketPolicy` | `listObjects` |
| `getBucketTags` | `presignedPostPolicy` |
| `getDefaultRetention` | `putObject` |
| `isVersioningEnabled` | `removeObject` |
| `listBuckets` | `removeObjects` |
| `listenBucketNotification` | `selectObjectContent` |
| `listIncompleteUploads` | `setObjectRetention` |
| `makeBucket` | `setObjectTags` |
| `removeBucket` | `statObject` |
| `removeIncompleteUpload` | `uploadObject` |
| `setBucketEncryption` |  |
| `setBucketLifeCycle` |  |
| `setBucketNotification` |  |
| `setBucketPolicy` |  |
| `setBucketTags` |  |
| `setDefaultRetention` |  |

### 1. Client Builder

Client Builder is used to create MinIO client. Builder has below methods to accept arguments.  
\| Method \| Description \|  
\|-----------------\|--------------------------------------------------------------------------------------------------------------------------------------------\|  
\| `endpoint()` \| Accepts endpoint as a String, URL or okhttp3.HttpUrl object and optionally accepts port number and flag to enable secure \(TLS\) connection. \|  
\| \| Endpoint as a string can be formatted like below: \|  
\| \| `https://storage-4175974314028753.cloud.bigconnect.io` \|  
\| `credentials()` \| Accepts access key \(aka user ID\) and secret key \(aka password\) of an account in S3 service. \|  
\| `region()` \| Accepts region name of S3 service. If specified, all operations use this region otherwise region is probed per bucket. \|  
\| `httpClient()` \| Custom HTTP client to override default. \|

**Examples**

```java
// 1. Create client to OSS service 'storage-4175974314028753.cloud.bigconnect.io' at port 443 with TLS security
// for anonymous access.
MinioClient minioClient = MinioClient.builder().endpoint("https://storage-4175974314028753.cloud.bigconnect.io").build();

// 2. Create client to OSS service 'storage-4175974314028753.cloud.bigconnect.io' at port 443 with TLS security
// using URL object for anonymous access.
MinioClient minioClient = MinioClient.builder().endpoint(new URL("https://storage-4175974314028753.cloud.bigconnect.io")).build();

// 3. Create client to OSS service 'storage-4175974314028753.cloud.bigconnect.io' at port 443 with TLS security
// for authenticated access.
MinioClient minioClient =
    MinioClient.builder()
        .endpoint("https://storage-4175974314028753.cloud.bigconnect.io")
        .credentials("Q3AM3UQ867SPQQA43P2F", "zuf+tfteSlswRu7BJ86wekitnifILbZam1KYY3TG")
        .build();


// 4. Create client to OSS service 'storage-4175974314028753.cloud.bigconnect.io' with TLS security,
// and custom HTTP client for authenticated access.
MinioClient minioClient =
    MinioClient.builder()
        .endpoint("https://storage-4175974314028753.cloud.bigconnect.io")
        .credentials("Q3AM3UQ867SPQQA43P2F", "zuf+tfteSlswRu7BJ86wekitnifILbZam1KYY3TG")
        .httpClient(customHttpClient)
        .build();
```

### Common Exceptions

All APIs throw below exceptions in addition to specific to API.

| Exception | Cause |
| :--- | :--- |
| ErrorResponseException | Thrown to indicate S3 service returned an error response. |
| IllegalArgumentException | Throws to indicate invalid argument passed. |
| InsufficientDataException | Thrown to indicate not enough data available in InputStream. |
| InternalException | Thrown to indicate internal library error. |
| InvalidBucketNameException | Thrown to indicate invalid bucket name passed. |
| InvalidKeyException | Thrown to indicate missing of HMAC SHA-256 library. |
| InvalidResponseException | Thrown to indicate S3 service returned invalid or no error response. |
| IOException | Thrown to indicate I/O error on S3 operation. |
| NoSuchAlgorithmException | Thrown to indicate missing of MD5 or SHA-256 digest library. |
| XmlParserException | Thrown to indicate XML parsing error. |

### 2. Bucket operations

#### bucketExists\(BucketExistsArgs args\)

`public boolean bucketExists(BucketExistsArgs args)` [_\[Javadoc\]_](http://minio.github.io/minio-java/io/minio/MinioClient.html#bucketExists-io.minio.BucketExistsArgs-)

Checks if a bucket exists.

**Parameters**  
\| Parameter \| Type \| Description \|  
\|:---------------\|:---------------------\|:---------------\|  
\| `bucketName` \| [_BucketExistsArgs_](http://minio.github.io/minio-java/io/minio/BucketExistsArgs.html) \| Arguments. \|

| Returns |
| :--- |
| _boolean_ - True if the bucket exists. |

**Example**

```java
// Check whether 'my-bucketname' exists or not.
boolean found = 
  minioClient.bucketExists(BucketExistsArgs.builder().bucket("my-bucketname").build());
if (found) {
  System.out.println("my-bucketname exists");
} else {
  System.out.println("my-bucketname does not exist");
}
```

#### deleteBucketEncryption\(DeleteBucketEncryptionArgs args\)

`private void deleteBucketEncryption(DeleteBucketEncryptionArgs args)` [_\[Javadoc\]_](http://minio.github.io/minio-java/io/minio/MinioClient.html#deleteBucketEncryption-io.minio.DeleteBucketEncryptionArgs-)

Deletes encryption configuration of a bucket.

**Parameters**  
\| Parameter \| Type \| Description \|  
\|:----------\|:-------------------------------\|:------------\|  
\| `args` \| [_DeleteBucketEncryptionArgs_](http://minio.github.io/minio-java/io/minio/DeleteBucketEncryptionArgs.html) \| Arguments. \|

**Example**

```java
minioClient.deleteBucketEncryption(
    DeleteBucketEncryptionArgs.builder().bucket("my-bucketname").build());
```

#### deleteBucketLifeCycle\(DeleteBucketLifeCycleArgs args\)

`private void deleteBucketLifeCycle(DeleteBucketLifeCycleArgs args)` [_\[Javadoc\]_](http://minio.github.io/minio-java/io/minio/MinioClient.html#deleteBucketLifeCycle-io.minio.DeleteBucketLifeCycleArgs-)

Deletes life-cycle configuration of a bucket.

**Parameters**  
\| Parameter \| Type \| Description \|  
\|:----------\|:------------------------------\|:------------\|  
\| `args` \| [_DeleteBucketLifeCycleArgs_](http://minio.github.io/minio-java/io/minio/DeleteBucketLifeCycleArgs.html) \| Arguments. \|

**Example**

```java
minioClient.deleteBucketLifeCycle(
    DeleteBucketLifeCycleArgs.builder().bucket("my-bucketname").build());
```

#### deleteBucketTags\(DeleteBucketTagsArgs args\)

`private void deleteBucketTags(DeleteBucketTagsArgs args)` [_\[Javadoc\]_](http://minio.github.io/minio-java/io/minio/MinioClient.html#deleteBucketTags-io.minio.DeleteBucketTagsArgs-)

Deletes tags of a bucket.

**Parameters**  
\| Parameter \| Type \| Description \|  
\|:----------\|:-------------------------\|:------------\|  
\| `args` \| [_DeleteBucketTagsArgs_](http://minio.github.io/minio-java/io/minio/DeleteBucketTagsArgs.html) \| Arguments. \|

**Example**

```java
minioClient.deleteBucketTags(DeleteBucketTagsArgs.builder().bucket("my-bucketname").build());
```

#### deleteBucketPolicy\(DeleteBucketPolicyArgs args\)

`private void deleteBucketPolicy(DeleteBucketPolicyArgs args)` [_\[Javadoc\]_](http://minio.github.io/minio-java/io/minio/MinioClient.html#deleteBucketPolicy-io.minio.DeleteBucketPolicyArgs-)

Deletes bucket policy configuration of a bucket.

**Parameters**  
\| Parameter \| Type \| Description \|  
\|:----------\|:---------------------------\|:------------\|  
\| `args` \| [_DeleteBucketPolicyArgs_](http://minio.github.io/minio-java/io/minio/DeleteBucketPolicyArgs.html) \| Argumnets. \|

**Example**

```java
minioClient.deleteBucketPolicy(DeleteBucketPolicyArgs.builder().bucket("my-bucketname").build());
```

#### deleteBucketNotification\(DeleteBucketNotificationArgs args\)

`public void deleteBucketNotification(DeleteBucketNotificationArgs args)` [_\[Javadoc\]_](http://minio.github.io/minio-java/io/minio/MinioClient.html#deleteBucketNotification-io.minio.DeleteBucketNotificationArgs-)

Deletes notification configuration of a bucket.

**Parameters**  
\| Parameter \| Type \| Description \|  
\|:----------\|:---------------------------------\|:------------\|opym  
\| `args` \| [_DeleteBucketNotificationArgs_](http://minio.github.io/minio-java/io/minio/DeleteBucketNotificationArgs.html) \| Arguments. \|

**Example**

```java
minioClient.deleteBucketNotification(
    DeleteBucketNotificationArgs.builder().bucket("my-bucketname").build());
```

#### deleteDefaultRetention\(DeleteDefaultRetentionArgs args\)

`public void deleteDefaultRetention(DeleteDefaultRetentionArgs args)` [_\[Javadoc\]_](http://minio.github.io/minio-java/io/minio/MinioClient.html#deleteDefaultRetention-io.minio.DeleteDefaultRetentionArgs-)

Deletes default object retention in a bucket.

**Parameters**  
\| Parameter \| Type \| Description \|  
\|:----------\|:-------------------------------\|:------------\|  
\| `args` \| [_DeleteDefaultRetentionArgs_](http://minio.github.io/minio-java/io/minio/DeleteDefaultRetentionArgs.html) \| Arguments. \|

**Example**

```java
minioClient.deleteDefaultRetention(
    DeleteDefaultRetentionArgs.builder().bucket("my-bucketname").build());
```

#### disableVersioning\(DisableVersioningArgs args\)

`public void disableVersioning(DisableVersioningArgs args)` [_\[Javadoc\]_](http://minio.github.io/minio-java/io/minio/MinioClient.html#disableVersioning-io.minio.DisableVersioningArgs-)

Disables object versioning feature in a bucket.

**Parameters**

| Parameter | Type | Description |
| :--- | :--- | :--- |
| `args` | [_DisableVersioningArgs_](http://minio.github.io/minio-java/io/minio/DisableVersioningArgs.html) | Arguments. |

**Example**

```java
minioClient.disableVersioning(DisableVersioningArgs.builder().bucket("my-bucket").build());
```

#### enableVersioning\(EnableVersioningArgs args\)

`public void enableVersioning(EnableVersioningArgs args)` [_\[Javadoc\]_](http://minio.github.io/minio-java/io/minio/MinioClient.html#enableVersioning-io.minio.EnableVersioningArgs-)

Enables object versioning feature in a bucket.

**Parameters**

| Parameter | Type | Description |
| :--- | :--- | :--- |
| `args` | [_EnableVersioningArgs_](http://minio.github.io/minio-java/io/minio/EnableVersioningArgs.html) | Arguments. |

**Example**

```java
minioClient.enableVersioning(EnableVersioningArgs.builder().bucket("my-bucket").build());
```

#### isVersioningEnabled\(IsVersioningEnabledArgs args\)

`public boolean isVersioningEnabled(IsVersioningEnabledArgs args)` [_\[Javadoc\]_](http://minio.github.io/minio-java/io/minio/MinioClient.html#isVersioningEnabled-io.minio.IsVersioningEnabledArgs-)

Get bucket version status.

**Parameters**

| Parameter | Type | Description |
| :--- | :--- | :--- |
| `args` | [_IsVersioningEnabledArgs_](http://minio.github.io/minio-java/io/minio/IsVersioningEnabledArgs.html) | Arguments. |

| Returns |
| :--- |
| _boolean_ - True if bucket versioning is enabled. |

**Example**

```java
boolean isVersioningEnabled =
  minioClient.isVersioningEnabled(
      IsVersioningEnabledArgs.builder().bucket("my-bucketname").build());
if (isVersioningEnabled) {
  System.out.println("Bucket versioning is enabled");
} else {
  System.out.println("Bucket versioning is disabled");
}
```

#### getBucketEncryption\(GetBucketEncryptionArgs args\)

`public SseConfiguration getBucketEncryption(GetBucketEncryptionArgs args)` [_\[Javadoc\]_](http://minio.github.io/minio-java/io/minio/MinioClient.html#getBucketEncryption-io.minio.GetBucketEncryptionArgs-)

Gets encryption configuration of a bucket.

**Parameters**  
\| Parameter \| Type \| Description \|  
\|:----------\|:----------------------------\|:------------\|  
\| `args` \| [_GetBucketEncryptionArgs_](http://minio.github.io/minio-java/io/minio/GetBucketEncryptionArgs.html) \| Arguments. \|

| Returns |
| :--- |
| [_SseConfiguration_](http://minio.github.io/minio-java/io/minio/messages/SseConfiguration.html) - Server-side encryption configuration. |

**Example**

```java
SseConfiguration config =
    minioClient.getBucketEncryption(
        GetBucketEncryptionArgs.builder().bucket("my-bucketname").build());
```

#### getBucketLifeCycle\(GetBucketLifeCycleArgs args\)

`public String getBucketLifeCycle(GetBucketLifeCycleArgs args)` [_\[Javadoc\]_](http://minio.github.io/minio-java/io/minio/MinioClient.html#getBucketLifeCycle-io.minio.GetBucketLifeCycleArgs-)

Gets life-cycle configuration of a bucket.

**Parameters**  
\| Parameter \| Type \| Description \|  
\|:----------\|:---------------------------\|:------------\|  
\| `args` \| [_GetBucketLifeCycleArgs_](http://minio.github.io/minio-java/io/minio/GetBucketLifeCycleArgs.html) \| Arguments. \|

| Returns |
| :--- |
| _String_ - Life-cycle configuration as XML string. |

**Example**

```java
String lifecycle =
    minioClient.getBucketLifecycle(
        GetBucketLifeCycleArgs.builder().bucket("my-bucketname").build());
System.out.println("Life cycle settings: " + lifecycle);
```

#### getBucketNotification\(GetBucketNotificationArgs args\)

`public NotificationConfiguration getBucketNotification(GetBucketNotificationArgs args)` [_\[Javadoc\]_](http://minio.github.io/minio-java/io/minio/MinioClient.html#getBucketNotification-io.minio.GetBucketNotificationArgs-)

Gets notification configuration of a bucket.

**Parameters**  
\| Parameter \| Type \| Description \|  
\|:----------\|:------------------------------\|:------------\|  
\| `args` \| [_GetBucketNotificationArgs_](http://minio.github.io/minio-java/io/minio/GetBucketNotificationArgs.html) \| Arguments. \|

| Returns |
| :--- |
| [_NotificationConfiguration_](http://minio.github.io/minio-java/io/minio/messages/NotificationConfiguration.html) - Notification configuration. |

**Example**

```java
NotificationConfiguration config =
    minioClient.getBucketNotification(
        GetBucketNotificationArgs.builder().bucket("my-bucketname").build());
```

#### getBucketPolicy\(GetBucketPolicyArgs args\)

`public String getBucketPolicy(GetBucketPolicyArgs args)` [_\[Javadoc\]_](http://minio.github.io/minio-java/io/minio/MinioClient.html#getBucketPolicy-io.minio.GetBucketPolicyArgs-)

Gets bucket policy configuration of a bucket.

**Parameters**  
\| Parameter \| Type \| Description \|  
\|:----------\|:------------------------\|:------------\|  
\| `args` \| [_GetBucketPolicyArgs_](http://minio.github.io/minio-java/io/minio/GetBucketPolicyArgs.html) \| Arguments. \|

| Returns |
| :--- |
| _String_ - Bucket policy configuration as JSON string. |

**Example**

```java
String config =
    minioClient.getBucketPolicy(GetBucketPolicyArgs.builder().bucket("my-bucketname").build());
```

#### getBucketTags\(GetBucketTagsArgs args\)

`public Tags getBucketTags(GetBucketTagsArgs args)` [_\[Javadoc\]_](http://minio.github.io/minio-java/io/minio/MinioClient.htmlgetBucketTags-io.minio.GetBucketTagsArgs-)

Gets tags of a bucket.

**Parameters**  
\| Parameter \| Type \| Description \|  
\|:----------\|:----------------------\|:------------\|  
\| `args` \| [_GetBucketTagsArgs_](http://minio.github.io/minio-java/io/minio/GetBucketTagsArgs.html) \| Arguments. \|

| Returns |
| :--- |
| [_Tags_](http://minio.github.io/minio-java/io/minio/messages/Tags.html) - tags. |

**Example**

```java
Tags tags = minioClient.getBucketTags(GetBucketTagsArgs.builder().bucket("my-bucketname").build());
```

#### getDefaultRetention\(GetDefaultRetentionArgs args\)

`public ObjectLockConfiguration getDefaultRetention(GetDefaultRetentionArgs args)` [_\[Javadoc\]_](http://minio.github.io/minio-java/io/minio/MinioClient.html#getDefaultRetention-io.minio.GetDefaultRetentionArgs-)

Gets default object retention in a bucket.

**Parameters**

| Parameter | Type | Description |
| :--- | :--- | :--- |
| `args` | [_GetDefaultRetentionArgs_](http://minio.github.io/minio-java/io/minio/GetDefaultRetentionArgs.html) | Arguments. |

| Returns |
| :--- |
| [_ObjectLockConfiguration_](http://minio.github.io/minio-java/io/minio/messages/ObjectLockConfiguration.html) - Default retention configuration. |

**Example**

```java
ObjectLockConfiguration config =
    minioClient.getDefaultRetention(
        GetDefaultRetentionArgs.builder().bucket("my-bucketname").build());
System.out.println("Mode: " + config.mode());
System.out.println("Duration: " + config.duration().duration() + " " + config.duration().unit());
```

#### listBuckets\(\)

`public List<Bucket> listBuckets()` [_\[Javadoc\]_](http://minio.github.io/minio-java/io/minio/MinioClient.html#listBuckets--)

Lists bucket information of all buckets.

| Returns |
| :--- |
| _List&lt;_[_Bucket_](http://minio.github.io/minio-java/io/minio/messages/Bucket.html)_&gt;_ - List of bucket information. |

**Example**

```java
List<Bucket> bucketList = minioClient.listBuckets();
for (Bucket bucket : bucketList) {
  System.out.println(bucket.creationDate() + ", " + bucket.name());
}
```

#### listenBucketNotification\(ListenBucketNotificationArgs args\)

`public CloseableIterator<Result<NotificationRecords>> listenBucketNotification(ListenBucketNotificationArgs args)` [_\[Javadoc\]_](http://minio.github.io/minio-java/io/minio/MinioClient.html#listenBucketNotification-io.minio.ListenBucketNotificationArgs-)

Listens events of object prefix and suffix of a bucket. The returned closable iterator is lazily evaluated hence its required to iterate to get new records and must be used with try-with-resource to release underneath network resources.

**Parameters**  
\| Parameter \| Type \| Description \|  
\|:----------\|:---------------------------------\|:------------\|  
\| `args` \| [_ListenBucketNotificationArgs_](http://minio.github.io/minio-java/io/minio/ListenBucketNotificationArgs.html) \| Arguments. \|

| Returns |
| :--- |
| [_CloseableIterator_](http://minio.github.io/minio-java/io/minio/CloseableIterator.html)_&lt;_[_Result_](http://minio.github.io/minio-java/io/minio/Result.html)_&lt;_[_NotificationRecords_](http://minio.github.io/minio-java/io/minio/messages/NotificationRecords.html)_&gt;&gt;_ - Lazy closable iterator contains event records. |

**Example**

```java
String[] events = {"s3:ObjectCreated:*", "s3:ObjectAccessed:*"};
try (CloseableIterator<Result<NotificationRecords>> ci =
    minioClient.listenBucketNotification(
        ListenBucketNotificationArgs.builder()
            .bucket("bucketName")
            .prefix("")
            .suffix("")
            .events(events)
            .build())) {
  while (ci.hasNext()) {
    NotificationRecords records = ci.next().get();
    for (Event event : records.events()) {
      System.out.println("Event " + event.eventType() + " occurred at " + event.eventTime()
          + " for " + event.bucketName() + "/" + event.objectName());
    }
  }
}
```

#### listIncompleteUploads\(ListIncompleteUploadsArgs args\)

`public Iterable<Result<Upload>> listIncompleteUploads(ListIncompleteUploadsArgs args)` [_\[Javadoc\]_](http://minio.github.io/minio-java/io/minio/MinioClient.html#listIncompleteUploads-io.minio.ListIncompleteUploadsArgs-)

Lists incomplete object upload information of a bucket.

**Parameters**

| Parameter | Type | Description |
| :--- | :--- | :--- |
| `args` | [_ListIncompleteUploadsArgs_](http://minio.github.io/minio-java/io/minio/ListIncompleteUploadsArgs.html) | Arguments. |

| Returns |
| :--- |
| _Iterable&lt;_[_Result_](http://minio.github.io/minio-java/io/minio/Result.html)_&lt;_[_Upload_](http://minio.github.io/minio-java/io/minio/messages/Upload.html)_&gt;&gt;_ - Lazy iterator contains object upload information. |

**Example**

```java
// Lists incomplete object upload information of a bucket.
Iterable<Result<Upload>> results =
    minioClient.listIncompleteUploads(
        ListIncompleteUploadsArgs.builder().bucket("my-bucketname").build());
for (Result<Upload> result : results) {
  Upload upload = result.get();
  System.out.println(upload.uploadId() + ", " + upload.objectName());
}

// Lists incomplete object upload information of a bucket for prefix.
Iterable<Result<Upload>> results =
    minioClient.listIncompleteUploads(
        ListIncompleteUploadsArgs.builder()
            .bucket("my-bucketname")
            .prefix("my-obj")
            .build());
for (Result<Upload> result : results) {
  Upload upload = result.get();
  System.out.println(upload.uploadId() + ", " + upload.objectName());
}

// Lists incomplete object upload information of a bucket for prefix recursively.
Iterable<Result<Upload>> results =
    minioClient.listIncompleteUploads(
        ListIncompleteUploadsArgs.builder()
            .bucket("my-bucketname")
            .prefix("my-obj")
            .recursive(true)
            .build());
for (Result<Upload> result : results) {
  Upload upload = result.get();
  System.out.println(upload.uploadId() + ", " + upload.objectName());
}

// Lists incomplete object upload information of a bucket for prefix, delimiter.
// uploadIdMarker and maxUpload to 500
Iterable<Result<Upload>> results =
    minioClient.listIncompleteUploads(
        ListIncompleteUploadsArgs.builder()
            .bucket("my-bucketname")
            .prefix("my-obj")
            .delimiter("-")
            .maxUploads(500)
            .uploadIdMarker("Xgw4MJT6ZPAVxpY0SAuGN7q4uWJJM22ZYg1W99trdp4tpO88")
            .build());
for (Result<Upload> result : results) {
  Upload upload = result.get();
  System.out.println(upload.uploadId() + ", " + upload.objectName());
```

#### listObjects\(ListObjectsArgs args\)

`public Iterable<Result<Item>> listObjects(ListObjectsArgs args)` [_\[Javadoc\]_](http://minio.github.io/minio-java/io/minio/MinioClient.html#listObjects-io.minio.ListObjectsArgs-)

Lists object information of a bucket.

**Parameters**  
\| Parameter \| Type \| Description \|  
\|:-----------------\|:--------------------\|:--------------------------\|  
\| `args` \| [_ListObjectsArgs_](http://minio.github.io/minio-java/io/minio/ListObjectsArgs.html) \| Arguments to list objects \|

| Returns |
| :--- |
| _Iterable&lt;_[_Result_](http://minio.github.io/minio-java/io/minio/Result.html)_&lt;_[_Item_](http://minio.github.io/minio-java/io/minio/messages/Item.html)_&gt;&gt;_ - Lazy iterator contains object information. |

**Example**

```java
// Lists objects information.
Iterable<Result<Item>> results = minioClient.listObjects(
    ListObjectsArgs.builder().bucket("my-bucketname").build());

// Lists objects information recursively.
Iterable<Result<Item>> results = minioClient.listObjects(
    ListObjectsArgs.builder().bucket("my-bucketname").recursive(true).build());

// Lists maximum 100 objects information those names starts with 'E' and after 'ExampleGuide.pdf'.
Iterable<Result<Item>> results = minioClient.listObjects(
    ListObjectsArgs.builder()
        .bucket("my-bucketname")
        .startAfter("ExampleGuide.pdf")
        .prefix("E")
        .maxKeys(100)
        .build());

// Lists maximum 100 objects information with version those names starts with 'E' and after
// 'ExampleGuide.pdf'.
Iterable<Result<Item>> results = minioClient.listObjects(
    ListObjectsArgs.builder()
        .bucket("my-bucketname")
        .startAfter("ExampleGuide.pdf")
        .prefix("E")
        .maxKeys(100)
        .includeVersions(true)
        .build());
```

#### makeBucket\(MakeBucketArgs args\)

`public void makeBucket(MakeBucketArgs args)` [_\[Javadoc\]_](http://minio.github.io/minio-java/io/minio/MinioClient.html#makeBucket-io.minio.MakeBucketArgs-)

Creates a bucket with given region and object lock feature enabled.

**Parameters**

| Parameter | Type | Description |
| :--- | :--- | :--- |
| `args` | [_MakeBucketArgs_](http://minio.github.io/minio-java/io/minio/MakeBucketArgs.html) | Arguments to create bucket |

**Example**

```java
// Create bucket with default region.
minioClient.makeBucket(
    MakeBucketArgs.builder()
        .bucket("my-bucketname")
        .build());

// Create bucket with specific region.
minioClient.makeBucket(
    MakeBucketArgs.builder()
        .bucket("my-bucketname")
        .region("us-west-1")
        .build());

// Create object-lock enabled bucket with specific region.
minioClient.makeBucket(
    MakeBucketArgs.builder()
        .bucket("my-bucketname")
        .region("us-west-1")
        .objectLock(true)
        .build());
```

#### removeBucket\(RemoveBucketArgs args\)

`public void removeBucket(RemoveBucketArgs args)` [_\[Javadoc\]_](http://minio.github.io/minio-java/io/minio/MinioClient.html#removeBucket-io.minio.RemoveBucketArgs-)

Removes an empty bucket.

**Parameters**

| Parameter | Type | Description |
| :--- | :--- | :--- |
| `args` | [_RemoveBucketArgs_](http://minio.github.io/minio-java/io/minio/RemoveBucketArgs.html) | Arguments. |

**Example**

```java
minioClient.removeBucket(RemoveBucketArgs.builder().bucket(bucketName).build());
```

#### removeIncompleteUpload\(RemoveIncompleteUploadArgs args\)

`public void removeIncompleteUpload(String bucketName, String objectName)` [_\[Javadoc\]_](http://minio.github.io/minio-java/io/minio/MinioClient.html#removeIncompleteUpload-io.minio.RemoveIncompleteUploadArgs-)

Removes incomplete uploads of an object.

**Parameters**  
\| Parameter \| Type \| Description \|  
\|:-----------------\|:-------------------------------\|:-----------------------------\|  
\| `args` \| [_RemoveIncompleteUploadArgs_](http://minio.github.io/minio-java/io/minio/RemoveIncompleteUploadArgs.html) \| Arguments. \|

**Example**

```java
minioClient.removeIncompleteUpload(
    RemoveIncompleteUploadArgs.builder()
        .bucket("my-bucketname")
        .object("my-objectname")
        .build());
```

#### setBucketEncryption\(SetBucketEncryptionArgs args\)

`public void setBucketEncryption(SetBucketEncryptionArgs args)` [_\[Javadoc\]_](http://minio.github.io/minio-java/io/minio/MinioClient.html#setBucketEncryption-io.minio.SetBucketEncryptionArgs-)

Sets encryption configuration of a bucket.

**Parameters**  
\| Parameter \| Type \| Description \|  
\|:----------\|:----------------------------\|:------------\|  
\| `args` \| [_SetBucketEncryptionArgs_](http://minio.github.io/minio-java/io/minio/SetBucketEncryptionArgs.html) \| Arguments. \|

**Example**

```java
minioClient.setBucketEncryption(
    SetBucketEncryptionArgs.builder().bucket("my-bucketname").config(config).build());
```

#### setBucketLifeCycle\(SetBucketLifeCycleArgs args\)

`public void setBucketLifeCycle(SetBucketLifeCycleArgs args)` [_\[Javadoc\]_](http://minio.github.io/minio-java/io/minio/MinioClient.html#setBucketLifeCycle-io.minio.SetBucketLifeCycleArgs-)

Sets life-cycle configuration to a bucket.

**Parameters**  
\| Parameter \| Type \| Description \|  
\|:----------\|:---------------------------\|:------------\|  
\| `args` \| [_SetBucketLifeCycleArgs_](http://minio.github.io/minio-java/io/minio/SetBucketLifeCycleArgs.html) \| Arguments. \|

**Example**

```java
// Lets consider variable 'lifeCycleXml' contains below XML String;
// <LifecycleConfiguration>
//   <Rule>
//     <ID>expire-bucket</ID>
//     <Prefix></Prefix>
//     <Status>Enabled</Status>
//     <Expiration>
//       <Days>365</Days>
//     </Expiration>
//   </Rule>
// </LifecycleConfiguration>
//
minioClient.setBucketLifecycle(
    SetBucketLifecycleArgs.builder().bucket("my-bucketname").config(lifeCycleXml).build());
```

#### setBucketNotification\(SetBucketNotificationArgs args\)

`public void setBucketNotification(SetBucketNotificationArgs args)` [_\[Javadoc\]_](http://minio.github.io/minio-java/io/minio/MinioClient.html#setBucketNotification-io.minio.SetBucketNotificationArgs-)

Sets notification configuration to a bucket.

**Parameters**

| Parameter | Type | Description |
| :--- | :--- | :--- |
| `args` | [_SetBucketNotificationArgs_](http://minio.github.io/minio-java/io/minio/SetBucketNotificationArgs.html) | Arguments. |

**Example**

```java
List<EventType> eventList = new LinkedList<>();
eventList.add(EventType.OBJECT_CREATED_PUT);
eventList.add(EventType.OBJECT_CREATED_COPY);

QueueConfiguration queueConfiguration = new QueueConfiguration();
queueConfiguration.setQueue("arn:minio:sqs::1:webhook");
queueConfiguration.setEvents(eventList);
queueConfiguration.setPrefixRule("images");
queueConfiguration.setSuffixRule("pg");

List<QueueConfiguration> queueConfigurationList = new LinkedList<>();
queueConfigurationList.add(queueConfiguration);

NotificationConfiguration config = new NotificationConfiguration();
config.setQueueConfigurationList(queueConfigurationList);

minioClient.setBucketNotification(
    SetBucketNotificationArgs.builder().bucket("my-bucketname").config(config).build());
```

#### setBucketPolicy\(SetBucketPolicyArgs args\)

`public void setBucketPolicy(SetBucketPolicyArgs args)` [_\[Javadoc\]_](http://minio.github.io/minio-java/io/minio/MinioClient.html#setBucketPolicy-io.minio.SetBucketPolicyArgs-)

Sets bucket policy configuration to a bucket.

**Parameters**

| Parameter | Type | Description |
| :--- | :--- | :--- |
| `args` | [_SetBucketPolicyArgs_](http://minio.github.io/minio-java/io/minio/SetBucketPolicyArgs.html) | Arguments. |

**Example**

```java
// Assume policyJson contains below JSON string;
// {
//     "Statement": [
//         {
//             "Action": [
//                 "s3:GetBucketLocation",
//                 "s3:ListBucket"
//             ],
//             "Effect": "Allow",
//             "Principal": "*",
//             "Resource": "arn:aws:s3:::my-bucketname"
//         },
//         {
//             "Action": "s3:GetObject",
//             "Effect": "Allow",
//             "Principal": "*",
//             "Resource": "arn:aws:s3:::my-bucketname/myobject*"
//         }
//     ],
//     "Version": "2012-10-17"
// }
//
minioClient.setBucketPolicy(
    SetBucketPolicyArgs.builder().bucket("my-bucketname").config(policyJson).build());
```

#### setBucketTags\(SetBucketTagsArgs args\)

`public void setBucketTags(SetBucketTagsArgs args)` [_\[Javadoc\]_](http://minio.github.io/minio-java/io/minio/MinioClient.html#setBucketTags-io.minio.SetBucketTagsArgs-)

Sets tags to a bucket.

**Parameters**

| Parameter | Type | Description |
| :--- | :--- | :--- |
| `args` | [_SetBucketTagsArgs_](http://minio.github.io/minio-java/io/minio/SetBucketTagsArgs.html) | Arguments. |

**Example**

```java
Map<String, String> map = new HashMap<>();
map.put("Project", "Project One");
map.put("User", "jsmith");
minioClient.setBucketTags(SetBucketTagsArgs.builder().bucket("my-bucketname").tags(map).build());
```

#### setDefaultRetention\(SetDefaultRetentionArgs args\)

`public void setDefaultRetention(SetDefaultRetentionArgs args)` [_\[Javadoc\]_](http://minio.github.io/minio-java/io/minio/MinioClient.html#setDefaultRetention-io.minio.SetDefaultRetentionArgs-)

Sets default object retention in a bucket.

**Parameters**  
\| Parameter \| Type \| Description \|  
\|:----------\|:----------------------------\|:------------\|  
\| `args` \| [_SetDefaultRetentionArgs_](http://minio.github.io/minio-java/io/minio/SetDefaultRetentionArgs.html) \| Arguments. \|

**Example**

```java
ObjectLockConfiguration config =
    new ObjectLockConfiguration(RetentionMode.COMPLIANCE, new RetentionDurationDays(100));
minioClient.setDefaultRetention(
    SetDefaultRetentionArgs.builder().bucket("my-bucketname").config(config).build());
```

### 3. Object operations

#### composeObject\(ComposeObjectArgs args\)

`public ObjectWriteResponse composeObject(ComposeObjectArgs args)` [_\[Javadoc\]_](http://minio.github.io/minio-java/io/minio/MinioClient.html#composeObject-io.minio.ComposeObjectArgs--)

Creates an object by combining data from different source objects using server-side copy.

**Parameters**  
\| Param \| Type \| Description \|  
\|:---------------\|:-------------------------\|:--------------\|  
\| `args` \| [_ComposeObjectArgs_](http://minio.github.io/minio-java/io/minio/ComposeObjectArgs.html) \| Arguments. \|

| Returns |
| :--- |
| [_ObjectWriteResponse_](http://minio.github.io/minio-java/io/minio/ObjectWriteResponse.html) - Contains information of created object. |

**Example**

```java
List<ComposeSource> sourceObjectList = new ArrayList<ComposeSource>();
sourceObjectList.add(
  ComposeSource.builder().bucket("my-job-bucket").object("my-objectname-part-one").build());
sourceObjectList.add(
  ComposeSource.builder().bucket("my-job-bucket").object("my-objectname-part-two").build());
sourceObjectList.add(
  ComposeSource.builder().bucket("my-job-bucket").object("my-objectname-part-three").build());

// Create my-bucketname/my-objectname by combining source object list.
minioClient.composeObject(
  ComposeObjectArgs.builder()
      .bucket("my-bucketname")
      .object("my-objectname")
      .sources(sourceObjectList)
      .build());

// Create my-bucketname/my-objectname with user metadata by combining source object
// list.
minioClient.composeObject(
    ComposeObjectArgs.builder()
      .bucket("my-bucketname")
      .object("my-objectname")
      .sources(sourceObjectList)
      .headers(Multimaps.forMap(userMetadata))
      .build());

// Create my-bucketname/my-objectname with user metadata and server-side encryption
// by combining source object list.
minioClient.composeObject(
  ComposeObjectArgs.builder()
      .bucket("my-bucketname")
      .object("my-objectname")
      .sources(sourceObjectList)
      .headers(Multimaps.forMap(userMetadata))
      .ssec(sse)
      .build());
```

#### copyObject\(CopyObjectArgs args\)

`public ObjectWriteResponse copyObject(CopyObjectArgs args)` [_\[Javadoc\]_](http://minio.github.io/minio-java/io/minio/MinioClient.html#copyObject-io.minio.CopyObjectArgs-)

Creates an object by server-side copying data from another object.

**Parameters**  
\| Parameter \| Type \| Description \|  
\|:----------\|:-------------------\|:------------\|  
\| `args` \| [_CopyObjectArgs_](http://minio.github.io/minio-java/io/minio/CopyObjectArgs.html) \| Arguments. \|

| Returns |
| :--- |
| [_ObjectWriteResponse_](http://minio.github.io/minio-java/io/minio/ObjectWriteResponse.html) - Contains information of created object. |

**Example**

```java
// Create object "my-objectname" in bucket "my-bucketname" by copying from object
// "my-objectname" in bucket "my-source-bucketname".
minioClient.copyObject(
    CopyObjectArgs.builder()
        .bucket("my-bucketname")
        .object("my-objectname")
        .srcBucket("my-source-bucketname")
        .build());

// Create object "my-objectname" in bucket "my-bucketname" by copying from object
// "my-source-objectname" in bucket "my-source-bucketname".
minioClient.copyObject(
    CopyObjectArgs.builder()
        .bucket("my-bucketname")
        .object("my-objectname")
        .srcBucket("my-source-bucketname")
        .srcObject("my-source-objectname")
        .build());

// Create object "my-objectname" in bucket "my-bucketname" with server-side encryption by
// copying from object "my-objectname" in bucket "my-source-bucketname".
minioClient.copyObject(
    CopyObjectArgs.builder()
        .bucket("my-bucketname")
        .object("my-objectname")
        .srcBucket("my-source-bucketname")
        .sse(sse)
        .build());

// Create object "my-objectname" in bucket "my-bucketname" by copying from SSE-C encrypted
// object "my-source-objectname" in bucket "my-source-bucketname".
minioClient.copyObject(
    CopyObjectArgs.builder()
        .bucket("my-bucketname")
        .object("my-objectname")
        .srcBucket("my-source-bucketname")
        .srcObject("my-source-objectname")
        .srcSsec(ssec)
        .build());

// Create object "my-objectname" in bucket "my-bucketname" with custom headers by copying from
// object "my-objectname" in bucket "my-source-bucketname" using conditions.
minioClient.copyObject(
    CopyObjectArgs.builder()
        .bucket("my-bucketname")
        .object("my-objectname")
        .srcBucket("my-source-bucketname")
        .headers(headers)
        .srcMatchETag(etag)
        .build());
```

#### deleteObjectTags\(DeleteObjectTagsArgs args\)

`private void deleteObjectTags(DeleteObjectTagsArgs args)` [_\[Javadoc\]_](http://minio.github.io/minio-java/io/minio/MinioClient.html#deleteObjectTags-io.minio.DeleteObjectTagsArgs-)

Deletes tags of an object.

**Parameters**  
\| Parameter \| Type \| Description \|  
\|:----------\|:-------------------------\|:------------\|  
\| `args` \| [_DeleteObjectTagsArgs_](http://minio.github.io/minio-java/io/minio/DeleteObjectTagsArgs.html) \| Arguments. \|

**Example**

```java
minioClient.deleteObjectTags(
    DeleteObjectArgs.builder().bucket("my-bucketname").object("my-objectname").build());
```

#### disableObjectLegalHold\(DisableObjectLegalHoldArgs args\)

`public void disableObjectLegalHold(DisableObjectLegalHoldArgs args)` [_\[Javadoc\]_](http://minio.github.io/minio-java/io/minio/MinioClient.html#disableObjectLegalHold-io.minio.DisableObjectLegalHoldArgs-)

Disables legal hold on an object.

**Parameters**

| Parameter | Type | Description |
| :--- | :--- | :--- |
| `args` | [_DisableObjectLegalHoldArgs_](http://minio.github.io/minio-java/io/minio/DisableObjectLegalHoldArgs.html) | Arguments. |

**Example**

```java
// Disables legal hold on an object.
minioClient.disableObjectLegalHold(
    DisableObjectLegalHoldArgs.builder()
        .bucket("my-bucketname")
        .object("my-objectname")
        .build());
```

#### enableObjectLegalHold\(EnableObjectLegalHoldArgs args\)

`public void enableObjectLegalHold(EnableObjectLegalHoldArgs args)` [_\[Javadoc\]_](http://minio.github.io/minio-java/io/minio/MinioClient.html#enableObjectLegalHold-io.minio.EnableObjectLegalHoldArgs-)

Enables legal hold on an object.

**Parameters**

| Parameter | Type | Description |
| :--- | :--- | :--- |
| `args` | [_EnableObjectLegalHoldArgs_](http://minio.github.io/minio-java/io/minio/EnableObjectLegalHoldArgs.html) | Argumments. |

**Example**

```java
// Disables legal hold on an object.
minioClient.enableObjectLegalHold(
    EnableObjectLegalHoldArgs.builder()
        .bucket("my-bucketname")
        .object("my-objectname")
        .build());
```

#### getObject\(GetObjectArgs args\)

`public InputStream getObject(GetObjectArgs args)` [_\[Javadoc\]_](http://minio.github.io/minio-java/io/minio/MinioClient.html#getObject-io.minio.GetObjectArgs-)

Gets data of an object. Returned `InputStream` must be closed after use to release network resources.

**Parameters**  
\| Parameter \| Type \| Description \|  
\|:---------------\|:----------------\|:---------------------------\|  
\| `args` \| _GetObjectArgs_ \| Arguments. \|

| Returns |
| :--- |
| _InputStream_ - Contains object data. |

**Example**

```java
// get object given the bucket and object name
try (InputStream stream = minioClient.getObject(
  GetObjectArgs.builder()
  .bucket("my-bucketname")
  .object("my-objectname")
  .build()) {
  // Read data from stream
}

// get object data from offset
try (InputStream stream = minioClient.getObject(
  GetObjectArgs.builder()
  .bucket("my-bucketname")
  .object("my-objectname")
  .offset(1024L)
  .build()) {
  // Read data from stream
}

// get object data from offset to length
try (InputStream stream = minioClient.getObject(
  GetObjectArgs.builder()
  .bucket("my-bucketname")
  .object("my-objectname")
  .offset(1024L)
  .length(4096L)
  .build()) {
  // Read data from stream
}

// get data of an SSE-C encrypted object
try (InputStream stream = minioClient.getObject(
  GetObjectArgs.builder()
  .bucket("my-bucketname")
  .object("my-objectname")
  .ssec(ssec)
  .build()) {
  // Read data from stream
}

// get object data from offset to length of an SSE-C encrypted object
try (InputStream stream = minioClient.getObject(
  GetObjectArgs.builder()
  .bucket("my-bucketname")
  .object("my-objectname")
  .offset(1024L)
  .length(4096L)
  .ssec(ssec)
  .build()) {
  // Read data from stream
}
```

#### downloadObject\(DownloadObjectArgs args\)

`public void downloadObject(DownloadObjectArgs args)` [_\[Javadoc\]_](http://minio.github.io/minio-java/io/minio/MinioClient.html#getObject-io.minio.DownloadObjectArgs-)

Downloads data of an object to file.

**Parameters**  
\| Parameter \| Type \| Description \|  
\|:-----------------\|:---------------------\|:-----------------------------\|  
\| `args` \| _DownloadObjectArgs_ \| Arguments. \|

**Example**

```java
// Download object given the bucket, object name and output file name
minioClient.downloadObject(
  DownloadObjectArgs.builder()
  .bucket("my-bucketname")
  .object("my-objectname")
  .fileName("my-object-file")
  .build());

// Download server-side encrypted object in bucket to given file name
minioClient.downloadObject(
  DownloadObjectArgs.builder()
  .bucket("my-bucketname")
  .object("my-objectname")
  .ssec(ssec)
  .fileName("my-object-file")
  .build());
```

#### getObjectRetention\(GetObjectRetentionArgs args\)

`public Retention getObjectRetention(GetObjectRetentionArgs args)` [_\[Javadoc\]_](http://minio.github.io/minio-java/io/minio/MinioClient.html#getObjectRetention-io.minio.GetObjectRetentionArgs-)

Gets retention configuration of an object.

**Parameters**

| Parameter | Type | Description |
| :--- | :--- | :--- |
| `args` | [_GetObjectRetentionArgs_](http://minio.github.io/minio-java/io/minio/GetObjectRetentionArgs.html) | Arguments. |

| Returns |
| :--- |
| [_Retention_](http://minio.github.io/minio-java/io/minio/messages/Retention.html) - Object retention configuration. |

**Example**

```java
// Object with version id.
Retention retention =
    minioClient.getObjectRetention(
        GetObjectRetentionArgs.builder()
            .bucket("my-bucketname")
            .object("my-objectname")
            .versionId("object-version-id")
            .build());
System.out.println("mode: " + retention.mode() + "until: " + retention.retainUntilDate());
```

#### getObjectTags\(GetObjectTagsArgs args\)

`public Tags getObjectTags(GetObjectTagsArgs args)` [_\[Javadoc\]_](http://minio.github.io/minio-java/io/minio/MinioClient.html#getObjectTags-io.minio.GetObjectTagsArgs-)

Gets tags of an object.

**Parameters**  
\| Parameter \| Type \| Description \|  
\|:----------\|:----------------------\|:------------\|  
\| `args` \| [_GetObjectTagsArgs_](http://minio.github.io/minio-java/io/minio/GetObjectTagsArgs.html) \| Arguments. \|

| Returns |
| :--- |
| [_Tags_](http://minio.github.io/minio-java/io/minio/messages/Tags.html) - tags. |

**Example**

```java
Tags tags = minioClient.getObjectTags(
    GetObjectTagsArgs.builder().bucket("my-bucketname").object("my-objectname").build());
```

#### getObjectUrl\(String bucketName, String objectName\)

`public String getObjectUrl(String bucketName, String objectName)` [_\[Javadoc\]_](http://minio.github.io/minio-java/io/minio/MinioClient.html#getObjectUrl-java.lang.String-java.lang.String-)

Gets URL of an object useful when this object has public read access.

**Parameters**  
\| Parameter \| Type \| Description \|  
\|:---------------\|:---------\|:---------------------------\|  
\| `bucketName` \| _String_ \| Name of the bucket. \|  
\| `objectName` \| _String_ \| Object name in the bucket. \|

| Returns |
| :--- |
| _String_ - URL string. |

**Example**

```java
String url = minioClient.getObjectUrl("my-bucketname", "my-objectname");
System.out.println("my-bucketname/my-objectname can be downloaded by " + url);
```

#### getPresignedObjectUrl\(GetPresignedObjectUrlArgs args\)

`public String getPresignedObjectUrl(GetPresignedObjectUrlArgs args)` [_\[Javadoc\]_](http://minio.github.io/minio-java/io/minio/MinioClient.html#getPresignedObjectUrl-io.minio.GetPresignedObjectUrlArgs-)

Gets presigned URL of an object for HTTP method, expiry time and custom request parameters.

**Parameters**  
\| Parameter \| Type \| Description \|  
\|:------------\|:-------------------------------\|:-------------\|  
\| `args` \| [_GetPresignedObjectUrlArgs_](http://minio.github.io/minio-java/io/minio/GetPresignedObjectUrlArgs.html) \| Arguments. \|

| Returns |
| :--- |
| _String_ - URL string. |

**Example**

```java
// Get presigned URL of an object for HTTP method, expiry time and custom request parameters.
String url =
    minioClient.getPresignedObjectUrl(
        GetPresignedObjectUrlArgs.builder()
            .method(Method.DELETE)
            .bucket("my-bucketname")
            .object("my-objectname")
            .expiry(24 * 60 * 60)
            .build());
System.out.println(url);

// Get presigned URL string to upload 'my-objectname' in 'my-bucketname' 
// with response-content-type as application/json and life time as one day.
Map<String, String> reqParams = new HashMap<String, String>();
reqParams.put("response-content-type", "application/json");

String url =
    minioClient.getPresignedObjectUrl(
        GetPresignedObjectUrlArgs.builder()
            .method(Method.PUT)
            .bucket("my-bucketname")
            .object("my-objectname")
            .expiry(1, TimeUnit.DAYS)
            .extraQueryParams(reqParams)
            .build());
System.out.println(url);

// Get presigned URL string to download 'my-objectname' in 'my-bucketname' and its life time
// is 2 hours.
String url =
    minioClient.getPresignedObjectUrl(
        GetPresignedObjectUrlArgs.builder()
            .method(Method.GET)
            .bucket("my-bucketname")
            .object("my-objectname")
            .expiry(2, TimeUnit.HOURS)
            .build());
System.out.println(url);
```

#### isObjectLegalHoldEnabled\(IsObjectLegalHoldEnabledArgs args\)

`public boolean isObjectLegalHoldEnabled(IsObjectLegalHoldEnabledArgs args)` [_\[Javadoc\]_](http://minio.github.io/minio-java/io/minio/MinioClient.html#isObjectLegalHoldEnabled-io.minio.IsObjectLegalHoldEnabledArgs-)

Returns true if legal hold is enabled on an object.

**Parameters**

| Parameter | Type | Description |
| :--- | :--- | :--- |
| `args` | [_IsObjectLegalHoldEnabledArgs_](http://minio.github.io/minio-java/io/minio/IsObjectLegalHoldEnabledArgs.html) | Arguments. |

| Returns |
| :--- |
| _boolean_ - True if legal hold is enabled. |

**Example**

```java
boolean status =
    s3Client.isObjectLegalHoldEnabled(
       IsObjectLegalHoldEnabledArgs.builder()
            .bucket("my-bucketname")
            .object("my-objectname")
            .versionId("object-versionId")
            .build());
if (status) {
  System.out.println("Legal hold is on");
else {
  System.out.println("Legal hold is off");
}
```

#### presignedPostPolicy\(PostPolicy policy\)

`public Map<String,String> presignedPostPolicy(PostPolicy policy)` [_\[Javadoc\]_](http://minio.github.io/minio-java/io/minio/MinioClient.html#presignedPostPolicy-io.minio.PostPolicy-)

Gets form-data of [PostPolicy](http://minio.github.io/minio-java/io/minio/PostPolicy.html) of an object to upload its data using POST method.

**Parameters**  
\| Parameter \| Type \| Description \|  
\|:-----------\|:---------------\|:--------------------------\|  
\| `policy` \| [_PostPolicy_](http://minio.github.io/minio-java/io/minio/PostPolicy.html) \| Post policy of an object. \|

| Returns |
| :--- |
| _Map_ - Contains form-data to upload an object using POST method. |

**Example**

```java
PostPolicy policy = new PostPolicy("my-bucketname", "my-objectname", ZonedDateTime.now().plusDays(7));

// 'my-objectname' should be 'image/png' content type
policy.setContentType("image/png");

// set success action status to 201 to receive XML document
policy.setSuccessActionStatus(201);

Map<String,String> formData = minioClient.presignedPostPolicy(policy);

// Print curl command to be executed by anonymous user to upload /tmp/userpic.png.
System.out.print("curl -X POST ");
for (Map.Entry<String,String> entry : formData.entrySet()) {
  System.out.print(" -F " + entry.getKey() + "=" + entry.getValue());
}
System.out.println(" -F file=@/tmp/userpic.png https://storage-4175974314028753.cloud.bigconnect.io/my-bucketname");
```

#### putObject\(PutObjectArgs args\)

`public ObjectWriteResponse putObject(PutObjectArgs args)` [_\[Javadoc\]_](http://minio.github.io/minio-java/io/minio/MinioClient.html#putObject-io.minio.PutObjectArgs-)

Uploads given stream as object in bucket.

**Parameters**  
\| Parameter \| Type \| Description \|  
\|:----------\|:------------------\|:------------\|  
\| `args` \| [_PutObjectArgs_](http://minio.github.io/minio-java/io/minio/PutObjectArgs.html) \| Arguments. \|

| Returns |
| :--- |
| [_ObjectWriteResponse_](http://minio.github.io/minio-java/io/minio/ObjectWriteResponse.html) - Contains information of created object. |

**Example**

```java
// Upload known sized input stream.
minioClient.putObject(
    PutObjectArgs.builder().bucket("my-bucketname").object("my-objectname").stream(
            inputStream, size, -1)
        .contentType("video/mp4")
        .build());

// Upload unknown sized input stream.
minioClient.putObject(
    PutObjectArgs.builder().bucket("my-bucketname").object("my-objectname").stream(
            inputStream, -1, 10485760)
        .contentType("video/mp4")
        .build());

// Create object ends with '/' (also called as folder or directory).
minioClient.putObject(
    PutObjectArgs.builder().bucket("my-bucketname").object("path/to/").stream(
            new ByteArrayInputStream(new byte[] {}), 0, -1)
        .build());

// Upload input stream with headers and user metadata.
Map<String, String> headers = new HashMap<>();
headers.put("X-Amz-Storage-Class", "REDUCED_REDUNDANCY");
Map<String, String> userMetadata = new HashMap<>();
userMetadata.put("My-Project", "Project One");
minioClient.putObject(
    PutObjectArgs.builder().bucket("my-bucketname").object("my-objectname").stream(
            inputStream, size, -1)
        .headers(headers)
        .userMetadata(userMetadata)
        .build());

// Upload input stream with server-side encryption.
minioClient.putObject(
    PutObjectArgs.builder().bucket("my-bucketname").object("my-objectname").stream(
            inputStream, size, -1)
        .sse(sse)
        .build());
```

#### uploadObject\(UploadObjectArgs args\)

`public void uploadObject(UploadObjectArgs args)` [_\[Javadoc\]_](http://minio.github.io/minio-java/io/minio/MinioClient.html#uploadObject-io.minio.UploadObjectArgs-)

Uploads contents from a file as object in bucket.

**Parameters**  
\| Parameter \| Type \| Description \|  
\|:----------\|:---------------------\|:------------\|  
\| `args` \| [_UploadObjectArgs_](http://minio.github.io/minio-java/io/minio/UploadObjectArgs.html) \| Arguments. \|

**Example**

```java
// Upload an JSON file.
minioClient.uploadObject(
    UploadObjectArgs.builder()
        .bucket("my-bucketname").object("my-objectname").filename("person.json").build());

// Upload a video file.
minioClient.uploadObject(
    UploadObjectArgs.builder()
        .bucket("my-bucketname")
        .object("my-objectname")
        .filename("my-video.avi")
        .contentType("video/mp4")
        .build());
```

#### removeObject\(RemoveObjectArgs args\)

`public void removeObject(RemoveObjectArgs args)` [_\[Javadoc\]_](http://minio.github.io/minio-java/io/minio/MinioClient.html#removeObject-io.minio.RemoveObjectArgs-)

Removes an object.

**Parameters**  
\| Parameter \| Type \| Description \|  
\|:----------\|:---------------------\|:------------\|  
\| `args` \| [_RemoveObjectArgs_](http://minio.github.io/minio-java/io/minio/RemoveObjectArgs.html) \| Arguments. \|

**Example**

```java
// Remove object.
minioClient.removeObject(
    RemoveObjectArgs.builder().bucket("my-bucketname").object("my-objectname").build());

// Remove versioned object.
minioClient.removeObject(
    RemoveObjectArgs.builder()
        .bucket("my-bucketname")
        .object("my-versioned-objectname")
        .versionId("my-versionid")
        .build());

// Remove versioned object bypassing Governance mode.
minioClient.removeObject(
    RemoveObjectArgs.builder()
        .bucket("my-bucketname")
        .object("my-versioned-objectname")
        .versionId("my-versionid")
        .bypassRetentionMode(true)
        .build());
```

#### removeObjects\(RemoveObjectsArgs args\)

`public Iterable<Result<DeleteError>> removeObjects(RemoveObjectsArgs args)` [_\[Javadoc\]_](http://minio.github.io/minio-java/io/minio/MinioClient.html#removeObjects-io.minio.RemoveObjectsArgs-)

Removes multiple objects lazily. Its required to iterate the returned Iterable to perform removal.

**Parameters**  
\| Parameter \| Type \| Description \|  
\|:----------\|:----------------------\|:------------\|  
\| `args` \| [_RemoveObjectsArgs_](http://minio.github.io/minio-java/io/minio/RemoveObjectsArgs.html) \| Arguments. \|

| Returns |
| :--- |
| _Iterable&lt;_[_Result_](http://minio.github.io/minio-java/io/minio/Result.html)_&lt;_[_DeleteError_](http://minio.github.io/minio-java/io/minio/messages/DeleteError.html)_&gt;&gt;_ - Lazy iterator contains object removal status. |

**Example**

```java
List<DeleteObject> objects = new LinkedList<>();
objects.add(new DeleteObject("my-objectname1"));
objects.add(new DeleteObject("my-objectname2"));
objects.add(new DeleteObject("my-objectname3"));
Iterable<Result<DeleteError>> results =
    minioClient.removeObjects(
        RemoveObjectsArgs.builder().bucket("my-bucketname").objects(objects).build());
for (Result<DeleteError> result : results) {
  DeleteError error = result.get();
  System.out.println(
      "Error in deleting object " + error.objectName() + "; " + error.message());
}
```

#### selectObjectContent\(SelectObjectContentArgs args\)

`public SelectResponseStream selectObjectContent(SelectObjectContentArgs args)` [_\[Javadoc\]_](http://minio.github.io/minio-java/io/minio/MinioClient.html#selectObjectContent-io.minio.SelectObjectContentArgs-)

Selects content of a object by SQL expression.

**Parameters**

| Parameter | Type | Description |
| :--- | :--- | :--- |
| `args` | [_SelectObjectContentArgs_](http://minio.github.io/minio-java/io/minio/SelectObjectContentArgs.html) | Arguments. |

| Returns |
| :--- |
| [_SelectResponseStream_](http://minio.github.io/minio-java/io/minio/SelectResponseStream.html) - Contains filtered records and progress. |

**Example**

```java
String sqlExpression = "select * from S3Object";
InputSerialization is = new InputSerialization(null, false, null, null, FileHeaderInfo.USE, null, null, null);
OutputSerialization os = new OutputSerialization(null, null, null, QuoteFields.ASNEEDED, null);
SelectResponseStream stream =
    minioClient.selectObjectContent(
        SelectObjectContentArgs.builder()
            .bucket("my-bucketname")
            .object("my-objectName")
            .sqlExpression(sqlExpression)
            .inputSerialization(is)
            .outputSerialization(os)
            .requestProgress(true)
            .build());

byte[] buf = new byte[512];
int bytesRead = stream.read(buf, 0, buf.length);
System.out.println(new String(buf, 0, bytesRead, StandardCharsets.UTF_8));

Stats stats = stream.stats();
System.out.println("bytes scanned: " + stats.bytesScanned());
System.out.println("bytes processed: " + stats.bytesProcessed());
System.out.println("bytes returned: " + stats.bytesReturned());

stream.close();
```

#### setObjectRetention\(SetObjectRetentionArgs args\)

`public void setObjectLockRetention(SetObjectRetentionArgs)` [_\[Javadoc\]_](http://minio.github.io/minio-java/io/minio/MinioClient.html#setObjectRetention-io.minio.SetObjectRetentionArgs-)

Sets retention configuration to an object.

**Parameters**

| Parameter | Type | Description |
| :--- | :--- | :--- |
| `args` | [_SetObjectRetentionArgs_](http://minio.github.io/minio-java/io/minio/SetObjectRetentionArgs.html) | Arguments. |

**Example**

```java
Retention retention = new Retention(RetentionMode.COMPLIANCE, ZonedDateTime.now().plusYears(1));
minioClient.setObjectRetention(
    SetObjectRetentionArgs.builder()
        .bucket("my-bucketname")
        .object("my-objectname")
        .config(retention)
        .bypassGovernanceMode(true)
        .build());
```

#### setObjectTags\(SetObjectTagsArgs args\)

`public void setObjectTags(SetObjectTagsArgs args)` [_\[Javadoc\]_](http://minio.github.io/minio-java/io/minio/MinioClient.html#setObjectTags-io.minio.SetObjectTagsArgs-)

Sets tags to an object.

**Parameters**

| Parameter | Type | Description |
| :--- | :--- | :--- |
| `args` | [_SetObjectTagsArgs_](http://minio.github.io/minio-java/io/minio/SetObjectTagsArgs.html) | Arguments. |

**Example**

```java
Map<String, String> map = new HashMap<>();
map.put("Project", "Project One");
map.put("User", "jsmith");
minioClient.setObjectTags(
    SetObjectTagsArgs.builder().bucket("my-bucketname").object("my-objectname").tags(map).build());
```

#### statObject\(StatObjectArgs args\)

`public ObjectStat statObject(StatObjectArgs args)` [_\[Javadoc\]_](http://minio.github.io/minio-java/io/minio/MinioClient.html#statObject-io.minio.StatObjectArgs-)

Gets object information and metadata of an object.

**Parameters**  
\| Parameter \| Type \| Description \|  
\|:----------\|:-------------------\|:------------\|  
\| `args` \| [_StatObjectArgs_](http://minio.github.io/minio-java/io/minio/StatObjectArgs.html) \| Arguments. \|

| Returns |
| :--- |
| [_ObjectStat_](http://minio.github.io/minio-java/io/minio/ObjectStat.html) - Populated object information and metadata. |

**Example**

```java
// Get information of an object.
ObjectStat objectStat =
    minioClient.statObject(
        StatObjectArgs.builder().bucket("my-bucketname").object("my-objectname").build());

// Get information of SSE-C encrypted object.
ObjectStat objectStat =
    minioClient.statObject(
        StatObjectArgs.builder()
            .bucket("my-bucketname")
            .object("my-objectname")
            .ssec(ssec)
            .build());

// Get information of a versioned object.
ObjectStat objectStat =
    minioClient.statObject(
        StatObjectArgs.builder()
            .bucket("my-bucketname")
            .object("my-objectname")
            .versionId("version-id")
            .build());

// Get information of a SSE-C encrypted versioned object.
ObjectStat objectStat =
    minioClient.statObject(
        StatObjectArgs.builder()
            .bucket("my-bucketname")
            .object("my-objectname")
            .versionId("version-id")
            .ssec(ssec)
            .build());
```

