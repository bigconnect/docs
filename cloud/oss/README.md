# Object Store

The Object Storage Service \(OSS\) is a secure and highly reliable cloud storage service provided by the BigConnect Cloud. It enables you to store a large amount of data in the cloud.

The Object Storage Service \(OSS\) is included in the Core Data Lake service and is provisioned together with the Core service.

OSS supports RESTful API operations that are independent of the console. You can store and access any type of data anytime, anywhere, and from any application. 

You can use API operations and SDKs provided by the BigConnect Cloud to transfer large amounts of data in or out of BigConnect Cloud OSS. You can use OSS buckets to store any kind of files that will be used in other cloud applications or services. 

## Terms

**Bucket** 

The container used to store objects in OSS. Every object is contained in a bucket. You can configure various attributes of a bucket and ACL.  

**Object** 

The fundamental entities stored in OSS. Objects are also known as files. An object has metadata, data, and key. The key is the unique object name in a bucket. Object metadata describes the attributes of an object such as the last modification time and the object size. You can also specify custom metadata of an object. 

**Endpoint** 

The domain name used to access OSS. OSS provides external services by using HTTP RESTful API operations.  

**AccessKey pair** 

The key pair composed of an AccessKey ID and an AccessKey secret. The AccessKey pair is used to perform access identity verification. OSS uses an AccessKey pair, which includes an AccessKey ID and an AccessKey secret to implement symmetric encryption and verify the identity of a requester. The AccessKey ID is used to identify a user. The AccessKey secret is used to encrypt the signature string and for OSS to verify the signature string. The AccessKey secret must be kept confidential. 

## Pricing

Traditional storage providers require you to purchase a predetermined amount of storage and network transfer capacity. If the amount and capacity is exceeded, your service is disabled or you are charged excess fees. If you do not use the full capacity, you still pay as though you have used it all. 

OSS charges you only for what you actually use, without excess fees. As your business grows, you can enjoy the cost advantages of the flexible infrastructure from BigConnect Cloud, which adapts to meet your ever-changing requirements. 

