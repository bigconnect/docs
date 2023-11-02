# Basic Concepts

Before you use OSS, we recommend that you have a basic understanding of the following concepts.

### Bucket

A bucket is a container for objects stored in OSS. Every object is contained in a bucket. The data model structure of BigConnect Cloud OSS is flat instead of hierarchical.&#x20;

* All objects (files) are directly related to their corresponding buckets. Therefore, OSS lacks the hierarchical structure of directories and subfolders as in a file system.&#x20;
* A user can have multiple buckets.&#x20;
* A bucket name must be globally unique within a tenant.&#x20;
* A bucket can contain an unlimited number of objects.&#x20;

The naming conventions for buckets are as follows:&#x20;

* The bucket names must contain only lower-case letters, numbers, and hyphens (-).&#x20;
* The bucket names must start and end with a lower-case letter or number.&#x20;
* The bucket names must be at least 3 bytes and no more than 63 bytes in length.&#x20;

### Object

Objects, also known as files, are the fundamental entities stored in OSS. An object is composed of metadata, data, and key. The key is the unique object name in a bucket. Metadata defines the attributes of an object, such as the time last modified and the object size. You can also specify custom metadata of an object.&#x20;

The lifecycle of an object starts when it is uploaded, and ends when it is deleted. During the lifecycle, the object content cannot be changed. If you want to modify an object, you must upload a new object with the same name as the existing one to replace it. Therefore, unlike the file system, OSS does not allow users to modify objects directly.&#x20;

The naming conventions for objects are as follows:&#x20;

* The object names must use UTF-8 encoding.&#x20;
* The object names must be at least 1 byte and no more than 1023 bytes.&#x20;
* The object names cannot start with a backslash ( \ ) or a forward slash ( / )&#x20;

### Endpoint

An endpoint is the domain name used to access the OSS. OSS provides external services through HTTP RESTful APIs.

### AccessKey

An AccessKey (AK) is composed of an AccessKeyId and an AccessKeySecret. They work in pairs to perform access identity verification. OSS verifies the identity of a request sender by using the AccessKeyId/AccessKeySecret symmetric encryption method. The AccessKeyId is used to identify a user. The AccessKeySecret is used for the user to encrypt the signature and for OSS to verify the signature. The AccessKeySecret must be kept confidential. &#x20;

### Strong consistency

In OSS, object operations are atomic, which means operations are either successful or failed without an intermediate state. OSS will never write corrupted or partial data.&#x20;

Object operations in OSS are strongly consistent. For example, once a user receives an upload (PUT) success response, the object can be read immediately, and the data has already been written in triplicate. Therefore, OSS provides strong consistency for read-after-write. The same is true for the delete operations. Once a user deletes an object, the object becomes nonexistent immediately.&#x20;

### OSS vs File Systems

| Comparison item     | OSS                                                                                                                                                                                                                                                                                                                                                                     | File system                                                                                                                                                                                                      |
| ------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Data model**      | OSS is a distributed object storage service that uses a key-value pair format.                                                                                                                                                                                                                                                                                          | The file system is a hierarchical tree structure of directories that contain files.                                                                                                                              |
| **Data retrieval**  | <p>Objects are retrieved based on unique object names (keys). </p><p>Although users can use names like test1/test.jpg, this does not indicate that the object test.jpg is saved in a directory named test1. For OSS, test1/test.jpg and a.jpg have no essential difference. Similar amounts of resources are consumed during access to objects of different names. </p> | Files are retrieved based on their locations in directories.                                                                                                                                                     |
| **Advantage**       | OSS supports massive concurrent accesses, which means large volumes of unstructured data (such as images, videos, and documents) can be stored and retrieved without excessive use of resources.                                                                                                                                                                        | Folder operations such as renaming, moving, and deleting directories are quite easy, because data does not need to be copied and replaced.                                                                       |
| **Disadvantage**    | <p>The stored objects cannot be modified directly. </p><p>If you want to modify an object, you must upload the new object of the same name to replace the existing one. </p>                                                                                                                                                                                            | System performance depends on the capacity of a single device. The more files and directories that are created in the file system, the more resources are consumed, and the lengthier the user process becomes.  |

As a result, mapping OSS to a file system is not a recommended practice. When you use OSS, we recommend that you make full use of its advantages, including its massive data processing capabilities to store massive volumes of unstructured data, such as images, videos, and documents.

The mapping between OSS concepts and file system concepts is as follows:

| OSS                                 | File system                             |
| ----------------------------------- | --------------------------------------- |
| Object                              | File                                    |
| Bucket                              | Home directory                          |
| Region                              | NA                                      |
| Endpoint                            | NA                                      |
| AccessKey                           | NA                                      |
| NA                                  | Multilevel directory                    |
| GetService                          | Retrieving the list of home directories |
| GetBucket                           | Retrieving the list of files            |
| PutObject                           | Writing a file                          |
| AppendObject                        | Appending data to an existing file      |
| GetObject                           | Reading a file                          |
| DeleteObject                        | Deleting an object                      |
| NA                                  | Modifying file content                  |
| CopyObject (same target and source) | Modifying file attributes               |
| CopyObject                          | Copying a file                          |
| NA                                  | Renaming a file                         |
