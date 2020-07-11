# JavaScript SDK QuickStart

The JavaScript Client SDK provides simple APIs to access the OSS service.

This quickstart guide will show you how to install the client SDK and execute an example JavaScript program. For a complete list of APIs and examples, please take a look at the [JavaScript SDK Reference](javascript-sdk-reference.md) section.

This document assumes that you have a working [nodejs](http://nodejs.org/) setup in place.

### Download from NPM

```text
npm install --save minio
```

### Using with TypeScript

```text
npm install --save-dev @types/minio
```

### Initialize OSS Client

You need five items in order to connect to an OSS service.

| Params | Description |
| :--- | :--- |
| endPoint | URL to object storage service \(eg. `http://storage-4175974314028753.cloud.bigconnect.io`\) |
| port | TCP/IP port number. This input is optional. Default value set to 443 for HTTPs. |
| accessKey | Access key is like user ID that uniquely identifies your account. |
| secretKey | Secret key is the password to your account. |
| useSSL | Always set this value to 'true' to enable secure \(HTTPS\) access |

```javascript
var OSS = require('minio')

var ossClient = new OSS.Client({
    endPoint: 'storage-4175974314028753.cloud.bigconnect.io',
    port: 443,
    useSSL: true,
    accessKey: 'Q3AM3UQ867SPQQA43P2F',
    secretKey: 'zuf+tfteSlswRu7BJ86wekitnifILbZam1KYY3TG'
});
```

### Quick Start Example - File Uploader

This example program connects to an object storage server, makes a bucket on the server and then uploads a file to the bucket.

Make sure you have an OSS service running for this example. 

```javascript
var OSS = require('minio')

// Instantiate the minio client with the endpoint
// and access keys as shown below.
var ossClient = new OSS.Client({
    endPoint: 'storage-4175974314028753.cloud.bigconnect.io',
    port: 443,
    accessKey: '4175974314028753',
    secretKey: '4175974314028753'
});

// File that needs to be uploaded.
var file = '/tmp/photos-europe.tar'

// Make a bucket called europetrip.
ossClient.makeBucket('europetrip', 'default', function(err) {
    if (err) return console.log(err)

    console.log('Bucket created successfully.')

    var metaData = {
        'Content-Type': 'application/octet-stream',
        'X-Amz-Meta-Testing': 1234,
        'example': 5678
    }
    // Using fPutObject API upload your file to the bucket europetrip.
    ossClient.fPutObject('europetrip', 'photos-europe.tar', file, metaData, function(err, etag) {
      if (err) return console.log(err)
      console.log('File uploaded successfully.')
    });
});

```

### Run file-uploader

```javascript
node file-uploader.js
Bucket created successfully.

osscli ls play/europetrip/
[2016-05-25 23:49:50 PDT]  17MiB photos-europe.tar
```

Have a look at the [JavaScript SDK Reference](javascript-sdk-reference.md) for more information.

