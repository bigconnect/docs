# Python SDK QuickStart

This quickstart guide will show you how to install the client SDK and execute an example python program. For a complete list of APIs and examples, please take a look at the Python SDK Reference section.

This document assumes that you have a working [Python](https://www.python.org/downloads/) setup in place.

### Minimum Requirements

* Python 2.7 or higher

### Download from pip

```text
pip install minio
```

### Download from pip3

```text
pip3 install minio
```

### Initialize OSS Client

You need four items in order to connect to OSS service.

| Params | Description |
| :--- | :--- |
| endpoint | URL to object storage service. |
| access\_key | Access key is like user ID that uniquely identifies your account. |
| secret\_key | Secret key is the password to your account. |
| secure | Set this value to 'True' to enable secure \(HTTPS\) access. |

```text
from minio import Minio
from minio.error import ResponseError

minioClient = Minio('storage-4175974314028753.cloud.bigconnect.io',
                  access_key='Q3AM3UQ867SPQQA43P2F',
                  secret_key='zuf+tfteSlswRu7BJ86wekitnifILbZam1KYY3TG',
                  secure=True)
```

**NOTE on concurrent usage:** The `Minio` object is thread safe when using the Python `threading` library. Specifically, it is **NOT** safe to share it between multiple processes, for example when using `multiprocessing.Pool`. The solution is simply to create a new `Minio` object in each process, and not share it between processes.

### Quick Start Example - File Uploader

This example program connects to a OSS Service, makes a bucket on the server and then uploads a file to the bucket.

**file-uploader.py**

```text
Copy# Import MinIO library.
from minio import Minio
from minio.error import (ResponseError, BucketAlreadyOwnedByYou,
                         BucketAlreadyExists)

# Initialize minioClient with an endpoint and access/secret keys.
minioClient = Minio('storage-4175974314028753.cloud.bigconnect.io',
                    access_key='Q3AM3UQ867SPQQA43P2F',
                    secret_key='zuf+tfteSlswRu7BJ86wekitnifILbZam1KYY3TG',
                    secure=True)

# Make a bucket with the make_bucket API call.
try:
       minioClient.make_bucket("maylogs", location="us-east-1")
except BucketAlreadyOwnedByYou as err:
       pass
except BucketAlreadyExists as err:
       pass
except ResponseError as err:
       raise

# Put an object 'pumaserver_debug.log' with contents from 'pumaserver_debug.log'.
try:
       minioClient.fput_object('maylogs', 'pumaserver_debug.log', '/tmp/pumaserver_debug.log')
except ResponseError as err:
       print(err)

```

**Run file-uploader**

```text
Copypython file_uploader.py

osscli ls play/maylogs/
[2016-05-27 16:41:37 PDT]  12MiB pumaserver_debug.log
```

Have a look at the [Python SDK Reference](oss-python-sdk-reference.md) for more information.

