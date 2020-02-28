# Quick Start

Alluxio needs to be formatted before starting the process. The following command formats the Alluxio journal and worker storage directories.

```text
$ /opt/bdl/bin/alluxio format
```

By default, Alluxio is configured to start a master and worker process when running locally. Start Alluxio on localhost with the following command:

The [Alluxio shell](https://docs.alluxio.io/os/user/stable/en/basic/Command-Line-Interface.html) provides command line operations for interacting with Alluxio. To see a list of filesystem operations, run

```text
$ /opt/bdl/bin/alluxio fs
```

List files in Alluxio with the `ls` command. To list all files in the root directory, use the following command:

```text
$ /opt/bdl/bin/alluxio fs ls /
```

At this moment, there are no files in Alluxio. Copy a file into Alluxio by using the `copyFromLocal` shell command.

```text
$ /opt/bdl/bin/alluxio fs copyFromLocal LICENSE /LICENSE
```

List the files in Alluxio again to see the `LICENSE` file.

```text
$ /opt/bdl/bin/alluxio fs ls /

-rw-r--r-- staff  staff     26847 NOT_PERSISTED 01-09-2018 15:24:37:088 100% /LICENSE
```

The output shows the file that exists in Alluxio, as well the size of the file, the date it was created, the owner and group of the file, and the percentage of the file that is cached in Alluxio.

The `cat` command prints the contents of the file.

```text
$ /opt/bdl/bin/alluxio fs cat /LICENSE

                               Apache License
                           Version 2.0, January 2004
                        http://www.apache.org/licenses/

   TERMS AND CONDITIONS FOR USE, REPRODUCTION, AND DISTRIBUTION
...
```

With the default configuration, Alluxio uses the local file system as its under file storage \(UFS\). The default path for the UFS is `./underFSStorage`. Examine the contents of the UFS with:

Note that the directory does not exist. This is because Alluxio is currently writing data only into Alluxio space, not to the UFS.

Configure Alluxio to persist the file from Alluxio space to the UFS by using the `persist` command.

```text
$ /opt/bdl/bin/alluxio fs persist /LICENSE
```

The file should appear when examining the UFS path again.

## Mounting remote objects

Alluxio unifies access to storage systems with the unified namespace feature. Read the [Unified Namespace blog post](https://www.alluxio.io/resources/whitepapers/unified-namespace-allowing-applications-to-access-data-anywhere/) and the [unified namespace documentation](https://docs.alluxio.io/os/user/stable/en/advanced/Namespace-Management.html) for more detailed explanations of the feature.

This feature allows users to mount different storage systems into the Alluxio namespace and access the files across various storage systems through the Alluxio namespace seamlessly.

Create a directory in Alluxio to store our mount points.

```text
$ /opt/bdl/bin/alluxio fs mkdir /mnt
```

Mount an existing S3 bucket to Alluxio. This guide uses the `alluxio-quick-start` S3 bucket.

```text
$ /opt/bdl/bin/alluxio fs mount --readonly alluxio://localhost:19998/mnt/s3 s3://alluxio-quick-start/data

Mounted s3://alluxio-quick-start/data at alluxio://localhost:19998/mnt/s3
```

List the files mounted from S3 through the Alluxio namespace by using the `ls` command.

```text
$ /opt/bdl/bin/alluxio fs ls /mnt/s3

-r-x------ staff  staff    955610 PERSISTED 01-09-2018 16:35:00:882   0% /mnt/s3/sample_tweets_1m.csv
-r-x------ staff  staff  10077271 PERSISTED 01-09-2018 16:35:00:910   0% /mnt/s3/sample_tweets_10m.csv
-r-x------ staff  staff     89964 PERSISTED 01-09-2018 16:35:00:972   0% /mnt/s3/sample_tweets_100k.csv
-r-x------ staff  staff 157046046 PERSISTED 01-09-2018 16:35:01:002   0% /mnt/s3/sample_tweets_150m.csv
```

The newly mounted files and directories are also visible in the [Alluxio web UI](http://localhost:19999/browse?path=%2Fmnt%2Fs3).

With Alluxio’s unified namespace, users can interact with data from different storage systems seamlessly. The `ls -R` command recursively lists all the files that exist under a directory.

```text
$ /opt/bdl/bin/alluxio fs ls -R /

-rw-r--r-- staff  staff     26847 PERSISTED 01-09-2018 15:24:37:088 100% /LICENSE
drwxr-xr-x staff  staff         1 PERSISTED 01-09-2018 16:05:59:547  DIR /mnt
dr-x------ staff  staff         4 PERSISTED 01-09-2018 16:34:55:362  DIR /mnt/s3
-r-x------ staff  staff    955610 PERSISTED 01-09-2018 16:35:00:882   0% /mnt/s3/sample_tweets_1m.csv
-r-x------ staff  staff  10077271 PERSISTED 01-09-2018 16:35:00:910   0% /mnt/s3/sample_tweets_10m.csv
-r-x------ staff  staff     89964 PERSISTED 01-09-2018 16:35:00:972   0% /mnt/s3/sample_tweets_100k.csv
-r-x------ staff  staff 157046046 PERSISTED 01-09-2018 16:35:01:002   0% /mnt/s3/sample_tweets_150m.csv
```

This shows all the files across all of the mounted storage systems. The `/LICENSE` file is from the local file system whereas the files under `/mnt/s3/` are in S3.

## Accelerating Data Access with Alluxio

Since Alluxio leverages memory to store data, it can accelerate access to data. Check the status of a file previously mounted from S3 into Alluxio:

```text
$ /opt/bdl/bin/alluxio fs ls /mnt/s3/sample_tweets_150m.csv

-r-x------ staff  staff 157046046 PERSISTED 01-09-2018 16:35:01:002   0% /mnt/s3/sample_tweets_150m.csv
```

The output shows that the file is **Not In Memory**. This file is a sample of tweets. Count the number of tweets with the word “kitten” and time the duration of the operation.

```text
$ time ./bin/alluxio fs cat /mnt/s3/sample_tweets_150m.csv | grep -c kitten

real	0m22.857s
user	0m7.557s
sys	  0m1.181s
```

Depending on your network connection, the operation may take over 20 seconds. If reading this file takes too long, use a smaller dataset. The other files in the directory are smaller subsets of this file. Alluxio can accelerate access to this data by using memory to store the data.

After reading the file by the `cat` command, check the status with the `ls` command:

```text
$ /opt/bdl/bin/alluxio fs ls /mnt/s3/sample_tweets_150m.csv
-r-x------ staff  staff 157046046 PERSISTED 01-09-2018 16:35:01:002 100% /mnt/s3/sample_tweets_150m.csv
```

The output shows that the file is now 100% loaded to Alluxio, so reading the file should be significantly faster.

Now count the number of tweets with the word “puppy”.

```text
$ time ./bin/alluxio fs cat /mnt/s3/sample_tweets_150m.csv | grep -c puppy

real	0m1.917s
user	0m2.306s
sys	  0m0.243s
```

Subsequent reads of the same file are noticeably faster since the data is stored in Alluxio memory.

Now count how many tweets mention the word “bunny”.

```text
$ time ./bin/alluxio fs cat /mnt/s3/sample_tweets_150m.csv | grep -c bunny

real	0m1.983s
user	0m2.362s
sys	  0m0.240s
```

Congratulations! You used Alluxio to accelerate access to data!

