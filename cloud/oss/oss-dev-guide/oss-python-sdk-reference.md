# Python SDK Reference

### Minimum Requirements

Java 1.8 or above, with one of the following environments:

* [OracleJDK 8.0](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
* [OpenJDK8.0](https://openjdk.java.net/install/)

### Download from maven

```text
Copy<dependency>
    <groupId>io.minio</groupId>
    <artifactId>minio</artifactId>
    <version>7.1.0</version>
</dependency>
```

### Download from gradle

```text
Copydependencies {
    compile 'io.minio:minio:7.1.0'
}
```

### Download from JAR

You can download the latest [JAR](https://repo1.maven.org/maven2/io/minio/minio/7.1.0/) directly from maven.

### Quick Start Example - File Uploader

This example program connects to an object storage server, makes a bucket on the server and then uploads a file to the bucket.

You need three items in order to connect to an object storage server.

| Params | Description |
| :--- | :--- |
| Endpoint | URL to object storage service. |
| Access Key | Access key is like user ID that uniquely identifies your account. |
| Secret Key | Secret key is the password to your account. |

Make sure you have an OSS service running for this example.

```java
import java.io.IOException;
import java.security.NoSuchAlgorithmException;
import java.security.InvalidKeyException;

import org.xmlpull.v1.XmlPullParserException;

import io.minio.MinioClient;
import io.minio.errors.MinioException;

public class FileUploader {
  public static void main(String[] args) throws NoSuchAlgorithmException, IOException, InvalidKeyException, XmlPullParserException {
    try {
      // Create a minioClient with the MinIO Server name, Port, Access key and Secret key.
      MinioClient minioClient = new MinioClient("https://storage-4175974314028753.cloud.bigconnect.io", "Q3AM3UQ867SPQQA43P2F", "zuf+tfteSlswRu7BJ86wekitnifILbZam1KYY3TG");

      // Check if the bucket already exists.
      boolean isExist = 
        minioClient.bucketExists(BucketExistsArgs.builder().bucket("asiatrip").build());
      if(isExist) {
        System.out.println("Bucket already exists.");
      } else {
        // Make a new bucket called asiatrip to hold a zip file of photos.
        minioClient.makeBucket(MakeBucketArgs.builder().bucket("asiatrip").build());
      }

      // Upload the zip file to the bucket with putObject
      minioClient.putObject("asiatrip","asiaphotos.zip", "/home/user/Photos/asiaphotos.zip", null);
      System.out.println("/home/user/Photos/asiaphotos.zip is successfully uploaded as asiaphotos.zip to `asiatrip` bucket.");
    } catch(MinioException e) {
      System.out.println("Error occurred: " + e);
    }
  }
}

```

**Compile FileUploader**

```text
Copyjavac -cp "minio-7.1.0-all.jar"  FileUploader.java
```

**Run FileUploader**

```text
Copyjava -cp "minio-7.1.0-all.jar:." FileUploader
/home/user/Photos/asiaphotos.zip is successfully uploaded as asiaphotos.zip to `asiatrip` bucket.

osscli ls play/asiatrip/
[2016-06-02 18:10:29 PDT]  82KiB asiaphotos.zip
```

Have a look at the [Java SDK Reference](oss-java-sdk-reference.md) for more information.

