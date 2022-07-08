# .NET SDK QuickStart

This document assumes that you have a working VisualStudio development environment.

### Minimum Requirements

* .NET 4.5.2, .NetStandard 2.0 or higher
* Visual Studio 2017

### Install from NuGet

To install [MinIO .NET package](https://www.nuget.org/packages/Minio/), run the following command in Nuget Package Manager Console.

```
PM> Install-Package Minio
```

### OSS Client Example

To connect to an OSS Service Instance, you will need to specify the following parameters.

| Parameter | Description                                                                         |
| --------- | ----------------------------------------------------------------------------------- |
| endpoint  | URL to object storage service. (eg. `storage-4175974314028753.cloud.bigconnect.io`) |
| accessKey | Access key is the user ID that uniquely identifies your account.                    |
| secretKey | Secret key is the password to your account.                                         |
| secure    | Enable/Disable HTTPS support. Must be set to True.                                  |

Example:

```csharp
using Minio;

// Initialize the client with access credentials.
private static MinioClient minio = new MinioClient("storage-4175974314028753.cloud.bigconnect.io",
                "Q3AM3UQ867SPQQA43P2F",
                "zuf+tfteSlswRu7BJ86wekitnifILbZam1KYY3TG"
                ).WithSSL();

// Create an async task for listing buckets.
var getListBucketsTask = minio.ListBucketsAsync();

// Iterate over the list of buckets.
foreach (Bucket bucket in getListBucketsTask.Result.Buckets)
{
    Console.WriteLine(bucket.Name + " " + bucket.CreationDateDateTime);
}

```

### Complete _File Uploader_ Example

This example program connects to an object storage server, creates a bucket and uploads a file to the bucket.\
To run the following example, click on \[Link] and start the project

```csharp
using System;
using Minio;
using Minio.Exceptions;
using Minio.DataModel;
using System.Threading.Tasks;

namespace FileUploader
{
    class FileUpload
    {
        static void Main(string[] args)
        {
            var endpoint  = "storage-4175974314028753.cloud.bigconnect.io";
            var accessKey = "Q3AM3UQ867SPQQA43P2F";
            var secretKey = "zuf+tfteSlswRu7BJ86wekitnifILbZam1KYY3TG";
            try
            {
                var minio = new MinioClient(endpoint, accessKey, secretKey).WithSSL();
                FileUpload.Run(minio).Wait();
            }
            catch (Exception ex)
            {
                Console.WriteLine(ex.Message);
            }
            Console.ReadLine();
        }

        // File uploader task.
        private async static Task Run(MinioClient minio)
        {
            var bucketName = "mymusic";
            var location   = "default";
            var objectName = "golden-oldies.zip";
            var filePath = "C:\\Users\\username\\Downloads\\golden_oldies.mp3";
            var contentType = "application/zip";

            try
            {
                // Make a bucket on the server, if not already present.
                bool found = await minio.BucketExistsAsync(bucketName);
                if (!found)
                {
                    await minio.MakeBucketAsync(bucketName, location);
                }
                // Upload a file to bucket.
                await minio.PutObjectAsync(bucketName, objectName, filePath, contentType);
                Console.WriteLine("Successfully uploaded " + objectName );
            }
            catch (MinioException e)
            {
                Console.WriteLine("File Upload Error: {0}", e.Message);
            }
        }
    }
}
```

### Running Examples

**On Windows**

* Clone this repository and open the Minio.Sln in Visual Studio 2017.
* Enter your credentials and bucket name, object name etc.in Minio.Examples/Program.cs\
  Uncomment the example test cases such as below in Program.cs to run an example.

```csharp
//Cases.MakeBucket.Run(minioClient, bucketName).Wait();
```

* Run the Minio.Client.Examples project from Visual Studio #### On Linux (Ubuntu 16.04)

**Setting up Mono and .NETCore on Linux**

> NOTE: minio-dotnet requires mono 5.0.1 stable release and .NET Core 2.0 SDK to build on Linux.

* Install [.NETCore](https://www.microsoft.com/net/core#linuxredhat) and [Mono](http://www.mono-project.com/download/#download-lin) for your distro. See sample script to install .NETCore and Mono for Ubuntu Xenial [mono\_install.sh](https://github.com/minio/minio-dotnet/blob/master/mono\_install.sh)

```
$ ./mono_install.sh
```

**Running Minio.Examples**

* Clone this project.

```
$ git clone https://github.com/minio/minio-dotnet && cd minio-dotnet
```

* Enter your credentials and bucket name, object name etc. in Minio.Examples/Program.cs Uncomment the example test cases such as below in Program.cs to run an example.

```csharp
//Cases.MakeBucket.Run(minioClient, bucketName).Wait();
```

```bash
$ cd Minio.Examples
$ dotnet build -c Release
$ dotnet run
```

Have a look at the [.NET SDK Reference](oss-dotnet-sdk-reference.md) for more information.
