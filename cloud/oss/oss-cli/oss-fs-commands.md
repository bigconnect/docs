# FS Commands

### ls

`ls` command lists files, buckets and objects. Use `--incomplete` flag to list partially copied content.

```text
USAGE:
   osscli ls [FLAGS] TARGET [TARGET ...]

FLAGS:
  --recursive, -r               list recursively
  --incomplete, -I              list incomplete uploads
  --help, -h                    show help
```

_Example: List all buckets in alias play:_

```text
osscli ls play
[2016-04-08 03:56:14 IST]     0B albums/
[2016-04-04 16:11:45 IST]     0B backup/
[2016-04-01 20:10:53 IST]     0B deebucket/
[2016-03-28 21:53:49 IST]     0B guestbucket/
[2016-04-08 20:58:18 IST]     0B mybucket/
```

### tree

`tree` command lists buckets and directories in a tree format. Use `--files` flag to include files/objects in listing.

```text
CopyUSAGE:
   osscli tree [FLAGS] TARGET [TARGET ...]

FLAGS:
  --help, -h                    show help
  --files, -f                   include files in tree
  --depth, -d                   set the maximum depth of the tree
```

_Example: List all buckets on play/test-bucket in a tree format._

```text
osscli tree play/test-bucket
play/test-bucket/
├─ dir_a
├─ dir_b
│  └─ dir_bb
└─ dir_x
   └─ dir_xx
```

### mb

`mb` command creates a new bucket on an object storage. On a filesystem, it behaves like `mkdir -p` command. Bucket is equivalent of a drive or mount point in filesystems and should not be treated as folders. OSS does not place any limits on the number of buckets created per user.

```text
USAGE:
   osscli mb [FLAGS] TARGET [TARGET...]

FLAGS:
  --ignore-existing, -p         ignore if bucket/directory already exists
  --with-lock, -l               enable object lock
  --help, -h                    show help

```

_Example: Create a new bucket named "mybucket" in alias play:_

```text
osscli mb play/mybucket
Bucket created successfully ‘play/mybucket’.
```

### rb

`rb` command removes a bucket and all its contents on an object storage. On a filesystem, it behaves like `rmdir` command.

Note that when a bucket is removed all policies associated with the bucket will also be removed. If you would like to just empty the objects in a bucket use `rm` command

```text
USAGE:
   osscli rb [FLAGS] TARGET [TARGET...]

FLAGS:
  --force                       allow a recursive remove operation
  --dangerous                   allow site-wide removal of objects
  --help, -h                    show help

```

_Example: Remove a bucket named "mybucket" in alias play:_

```text
osscli rb play/mybucket --force
Bucket removed successfully ‘play/mybucket’.
```

### cat

`cat` command concatenates contents of a file or object to another. You may also use it to simply display the contents to stdout

```text
USAGE:
   osscli cat [FLAGS] SOURCE [SOURCE...]

FLAGS:
  --encrypt-key value           encrypt/decrypt objects (using server-side encryption with customer provided keys)
  --help, -h                    show help

ENVIRONMENT VARIABLES:
   OSS_ENCRYPT_KEY:  list of comma delimited prefix=secret values
```

_Example: Display the contents of a text file `myobject.txt`_

```text
osscli cat play/mybucket/myobject.txt
Hello OSS!!
```

_Example: Display the contents of a server encrypted object `myencryptedobject.txt`_

```text
osscli cat --encrypt-key "play/mybucket=32byteslongsecretkeymustbegiven1" play/mybucket/myencryptedobject.txt
Hello OSS!!
```

_Example: Display the contents of a server encrypted object `myencryptedobject.txt`. Pass base64 encoded string if encryption key contains non-printable character like tab_

```text
Copymc cat --encrypt-key "play/mybucket=MzJieXRlc2xvbmdzZWNyZWFiY2RlZmcJZ2l2ZW5uMjE=" play/mybucket/myencryptedobject.txt
Hello OSS!!
```

### sql

`sql` run sql queries on objects.

```text
USAGE:
  osscli sql [FLAGS] TARGET [TARGET...]

FLAGS:
  --query value, -e value       sql query expression
  --recursive, -r               sql query recursively
  --csv-input value             csv input serialization option
  --json-input value            json input serialization option
  --compression value           input compression type
  --csv-output value            csv output serialization option
  --json-output value           json output serialization option
  --encrypt-key value           encrypt/decrypt objects (using server-side encryption with customer provided keys)
  --help, -h                    show help

ENVIRONMENT VARIABLES:
   OSS_ENCRYPT_KEY:  list of comma delimited prefix=secret values

INPUT SERIALIZATION
  --csv-input or --json-input can be used to specify input data format. Format is
  specified by a string with pattern "key=value,..." for valid key(s).

  DATA FORMAT:
    csv: Use --csv-input flag
      Valid keys:
        RecordDelimiter (rd)
        FieldDelimiter (fd)
        QuoteChar (qc)
        QuoteEscChar (qec)
        FileHeader (fh)
        Comments (cc)
        QuotedRecordDelimiter (qrd)

    json: Use --json-input flag
      Valid keys:
        Type
    parquet: If object name ends in .parquet, this is automatically interpreted.

OUTPUT SERIALIZATION
  --csv-output or --json-output can be used to specify output data format. Format is
  specified by a string with pattern "key=value,..." for valid key(s).
  DATA FORMAT:
    csv: Use --csv-output flag
      Valid keys:
        RecordDelimiter (rd)
        FieldDelimiter (fd)
        QuoteChar (qc)
        QuoteEscChar (qec)
        QuoteFields (qf)

    json: Use --json-output flag
      Valid keys:
        RecordDelimiter (rd)

COMPRESSION TYPE
    --compression specifies if the queried object is compressed.
    Valid values: NONE | GZIP | BZIP2

```

_Example: Run an aggregation query on an object on OSS_

```text
osscli sql --query "select count(s.power) from S3Object" play/iot-devices/power-ratio.csv
```

_Example: Run an aggregation query on an encrypted object with customer provided keys_

```text
osscli sql --encrypt-key "play/iot-devices=32byteslongsecretkeymustbegiven1" \
    --query "select count(s.power) from S3Object" play/iot-devices/power-ratio-encrypted.csv
```

### head

`head` display first 'n' lines of an object

```text
USAGE:
   osscli head [FLAGS] SOURCE [SOURCE...]

FLAGS:
  -n value, --lines value       print the first 'n' lines (default: 10)
  --encrypt-key value           encrypt/decrypt objects (using server-side encryption with customer provided keys)
  --help, -h                    show help

ENVIRONMENT VARIABLES:
   OSS_ENCRYPT_KEY:  list of comma delimited prefix=secret values
```

_Example: Display the first line of a text file `myobject.txt`_

```text
osscli head -n 1 play/mybucket/myobject.txt
Hello!!
```

_Example: Display the first line of a server encrypted object `myencryptedobject.txt`_

```text
osscli head -n 1 --encrypt-key "play/mybucket=32byteslongsecretkeymustbegiven1" play/mybucket/myencryptedobject.txt
Hello!!
```

### lock

`lock` sets and gets object lock configuration

```text
USAGE:
   osscli lock [FLAGS] TARGET [governance | compliance] [VALIDITY]

FLAGS:
  --clear, -c                   clears previously stored object lock configuration
  --json                        enable JSON formatted output
  --help, -h                    show help
```

_Example: Set object lock configuration of 30 day compliance on bucket `mybucket`_

```text
osscli lock play/mybucket compliance 30d
```

_Example: Display the object lock configuration for bucket `mybucket`_

```text
osscli lock play/mybucket
COMPLIANCE mode is enabled for 30d
```

_Example: Display the object lock configuration for bucket `mybucket`_

```text
osscli lock play/mybucket
COMPLIANCE mode is enabled for 30d
```

_Example: Clear object lock configuration for bucket `mybucket`_

```text
osscli lock --clear play/mybucket
Object lock configuration cleared successfully
```

### retention

`retention` sets object retention for objects with a given prefix

```text
USAGE:
   osscli retention [FLAGS] TARGET [governance | compliance] [VALIDITY]

FLAGS:
  --bypass                      bypass governance
  --recursive, -r               apply retention recursively
  --json                        enable JSON formatted output
  --help, -h                    show help
```

_Example: Set governance for 30 days for object `prefix` on bucket `mybucket`_

```text
osscli retention play/mybucket/prefix governance 30d -r
Object retention successfully set for objects with prefix `play/mybucket/prefix`.

```

_Objects created with prefix `prefix` in the above bucket `mybucket` cannot be deleted until the compliance period is over_

```text
osscli cp ~/comp.csv play/mybucket/prefix/
mc rm play/mybucket/prefix/comp.csv
Removing `play/mybucket/prefix/comp.csv`.
mc: <ERROR> Failed to remove `play/mybucket/prefix/comp.csv`. Object is WORM protected and cannot be overwritten
```

### legalhold

`legalhold` sets object legal hold for objects

```text
USAGE:
   osscli legalhold [FLAGS] TARGET [ON | OFF]

FLAGS:
  --recursive, -r               apply legal hold recursively
  --json                        enable JSON formatted output
  --help, -h                    show help
```

_Example: Enable legal hold for objects with prefix `prefix` on bucket `mybucket`_

```text
osscli legalhold play/mybucket/prefix ON -r
Object legal hold successfully set for prefix `play/mybucket/prefix`.

```

_Objects created with prefix `prefix` in the above bucket `mybucket` cannot be deleted until the legal hold is lifted_

```text
osscli cp ~/test.csv play/mybucket/prefix/
osscli legalhold play/mybucket/prefix/test.csv ON
osscli rm play/mybucket/prefix/test.csv
Removing `play/mybucket/prefix/test.csv`.
mc: <ERROR> Failed to remove `play/mybucket/prefix/test.csv`. Object is WORM protected and cannot be overwritten
```

### pipe

`pipe` command copies contents of stdin to a target. When no target is specified, it writes to stdout.

```text
USAGE:
   osscli pipe [FLAGS] [TARGET]

FLAGS:
  --encrypt value               encrypt objects (using server-side encryption with server managed keys)
  --encrypt-key value           encrypt/decrypt objects (using server-side encryption with customer provided keys)
  --help, -h                    show help

ENVIRONMENT VARIABLES:
   OSS_ENCRYPT:      list of comma delimited prefix values
   OSS_ENCRYPT_KEY:  list of comma delimited prefix=secret values
```

_Example: Stream MySQL database dump to OSS directly._

```text
mysqldump -u root -p ******* accountsdb | osscli pipe play/sql-backups/backups/accountsdb-oct-9-2015.sql
```

### cp

`cp` command copies data from one or more sources to a target. All copy operations to object storage are verified with MD5SUM checksums. Interrupted or failed copy operations can be resumed from the point of failure.

```text
USAGE:
   osscli cp [FLAGS] SOURCE [SOURCE...] TARGET

FLAGS:
  --recursive, -r                    copy recursively
  --older-than value                 copy object(s) older than N days (default: 0)
  --newer-than value                 copy object(s) newer than N days (default: 0)
  --storage-class value, --sc value  set storage class for new object(s) on target
  --preserve,-a                      preserve file system attributes and bucket policy rules on target bucket(s)
  --attr                             add custom metadata for the object (format: KeyName1=string;KeyName2=string)
  --continue, -c                     create or resume copy session
  --encrypt value                    encrypt/decrypt objects (using server-side encryption with server managed keys)
  --encrypt-key value                encrypt/decrypt objects (using server-side encryption with customer provided keys)
  --help, -h                         show help

ENVIRONMENT VARIABLES:
   OSS_ENCRYPT:      list of comma delimited prefixes
   OSS_ENCRYPT_KEY:  list of comma delimited prefix=secret values
```

_Example: Copy a text file to an object storage._

```text
osscli cp myobject.txt play/mybucket
myobject.txt:    14 B / 14 B  ▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓  100.00 % 41 B/s 0
```

_Example: Copy a text file to an object storage with specified metadata._

```text
osscli cp --attr key1=value1;key2=value2 myobject.txt play/mybucket
myobject.txt:    14 B / 14 B  ▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓  100.00 % 41 B/s 0
```

_Example: Copy a folder recursively from OSS cloud storage to Amazon S3 cloud storage with specified metadata._

```text
osscli cp --attr Cache-Control=max-age=90000,min-fresh=9000\;key1=value1\;key2=value2 --recursive play/mybucket/burningman2011/ s3/mybucket/
https://storage-170393591705357.cloud.bigconnect.io/mybucket/myobject.txt:    14 B / 14 B  ▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓  100.00 % 41 B/s 0
```

_Example: Copy a text file to an object storage and assign storage-class `REDUCED_REDUNDANCY` to the uploaded object._

```text
osscli cp --storage-class REDUCED_REDUNDANCY myobject.txt play/mybucket
myobject.txt:    14 B / 14 B  ▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓  100.00 % 41 B/s 0
```

_Example: Copy a server-side encrypted file to an object storage._

```text
osscli cp --recursive --encrypt-key "s3/documents/=32byteslongsecretkeymustbegiven1 , play/documents/=32byteslongsecretkeymustbegiven2" s3/documents/myobject.txt play/documents/
myobject.txt:    14 B / 14 B  ▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓  100.00 % 41 B/s 0
```

_Example: Perform key-rotation on a server-side encrypted object_

```text
osscli cp --encrypt-key 'play1/mybucket=32byteslongsecretkeymustgenerate , play2/mybucket/=32byteslongsecretkeymustgenerat1' play1/mybucket/encryptedobject play2/mybucket/encryptedobject
encryptedobject:    14 B / 14 B  ▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓  100.00 % 41 B/s 0
```

Notice that two different aliases play1 and play2 are used for the same endpoint to provide the old secretkey and the newly rotated key.

_Example: Copy a javascript file to object storage and assign Cache-Control header to the uploaded object_

```text
osscli cp --attr Cache-Control=no-cache myscript.js play/mybucket
myscript.js:    14 B / 14 B  ▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓  100.00 % 41 B/s 0
```

_Example: Copy a text file to an object storage and preserve the filesyatem attributes._

```text
osscli cp -a myobject.txt play/mybucket
myobject.txt:    14 B / 14 B  ▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓  100.00 % 41 B/s 0
```

### mv

`mv` command movies data from one or more sources to a target. All move operations to object storage are verified with MD5SUM checksums. Interrupted or failed move operations can be resumed from the point of failure.

```text
USAGE:
   osscli mv [FLAGS] SOURCE [SOURCE...] TARGET

FLAGS:
  --recursive, -r                    move recursively
  --older-than value                 move object(s) older than N days (default: 0)
  --newer-than value                 move object(s) newer than N days (default: 0)
  --storage-class value, --sc value  set storage class for new object(s) on target
  --preserve,-a                      preserve file system attributes and bucket policy rules on target bucket(s)
  --attr                             add custom metadata for the object (format: KeyName1=string;KeyName2=string)
  --continue, -c                     create or resume move session
  --encrypt value                    encrypt/decrypt objects (using server-side encryption with server managed keys)
  --encrypt-key value                encrypt/decrypt objects (using server-side encryption with customer provided keys)
  --help, -h                         show help

ENVIRONMENT VARIABLES:
   OSS_ENCRYPT:      list of comma delimited prefixes
   OSS_ENCRYPT_KEY:  list of comma delimited prefix=secret values
```

_Example: Move a text file to an object storage._

```text
osscli mv myobject.txt play/mybucket
myobject.txt:    14 B / 14 B  ▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓  100.00 % 41 B/s 0
Waiting move operations to complete
```

_Example: Move a text file to an object storage with specified metadata._

```text
osscli mv --attr key1=value1;key2=value2 myobject.txt play/mybucket
myobject.txt:    14 B / 14 B  ▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓  100.00 % 41 B/s 0
Waiting move operations to complete
```

_Example: Move a folder recursively from OSS cloud storage to Amazon S3 cloud storage with specified metadata._

```text
osscli mv --attr Cache-Control=max-age=90000,min-fresh=9000\;key1=value1\;key2=value2 --recursive play/mybucket/burningman2011/ s3/mybucket/
https://storage-170393591705357.cloud.bigconnect.io/mybucket/myobject.txt:    14 B / 14 B  ▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓  100.00 % 41 B/s 0
Waiting move operations to complete
```

_Example: Move a text file to an object storage and assign storage-class `REDUCED_REDUNDANCY` to the uploaded object._

```text
osscli mv --storage-class REDUCED_REDUNDANCY myobject.txt play/mybucket
myobject.txt:    14 B / 14 B  ▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓  100.00 % 41 B/s 0
Waiting move operations to complete
```

_Example: Move a server-side encrypted file to an object storage._

```text
osscli mv --recursive --encrypt-key "s3/documents/=32byteslongsecretkeymustbegiven1 , play/documents/=32byteslongsecretkeymustbegiven2" s3/documents/myobject.txt play/documents/
myobject.txt:    14 B / 14 B  ▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓  100.00 % 41 B/s 0
Waiting move operations to complete
```

_Example: Perform key-rotation on a server-side encrypted object_

```text
osscli mv --encrypt-key 'play1/mybucket=32byteslongsecretkeymustgenerate , play2/mybucket/=32byteslongsecretkeymustgenerat1' play1/mybucket/encryptedobject play2/mybucket/encryptedobject
encryptedobject:    14 B / 14 B  ▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓  100.00 % 41 B/s 0
Waiting move operations to complete
```

Notice that two different aliases play1 and play2 are used for the same endpoint to provide the old secretkey and the newly rotated key.

_Example: Move a javascript file to object storage and assign Cache-Control header to the uploaded object_

```text
osscli mv --attr Cache-Control=no-cache myscript.js play/mybucket
myscript.js:    14 B / 14 B  ▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓  100.00 % 41 B/s 0
Waiting move operations to complete
```

_Example: Move a text file to an object storage and preserve the filesyatem attributes._

```text
osscli mv -a myobject.txt play/mybucket
myobject.txt:    14 B / 14 B  ▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓  100.00 % 41 B/s 0
Waiting move operations to complete
```

### rm

Use `rm` command to remove file or object

```text
USAGE:
   osscli rm [FLAGS] TARGET [TARGET ...]

FLAGS:
  --recursive, -r               remove recursively
  --force                       allow a recursive remove operation
  --dangerous                   allow site-wide removal of objects
  --incomplete, -I              remove incomplete uploads
  --fake                        perform a fake remove operation
  --stdin                       read object names from STDIN
  --older-than value            remove objects older than L days, M hours and N minutes LMN[d|h|m]. (default: 0)
  --newer-than value            remove objects newer than L days, M hours and N minutes LMN[d|h|m]. (default: 0)
  --encrypt-key value           encrypt/decrypt objects (using server-side encryption with customer provided keys)
  --help, -h                    show help

ENVIRONMENT VARIABLES:
   OSS_ENCRYPT_KEY:  list of comma delimited prefix=secret values
```

_Example: Remove a single object._

```text
osscli rm play/mybucket/myobject.txt
Removing `play/mybucket/myobject.txt`.
```

_Example: Remove an encrypted object._

```text
osscli rm --encrypt-key "play/mybucket=32byteslongsecretkeymustbegiven1" play/mybucket/myobject.txt
Removing `play/mybucket/myobject.txt`.
```

_Example: Recursively remove a bucket's contents. Since this is a dangerous operation, you must explicitly pass `--force` option._

```text
osscli rm --recursive --force play/mybucket
Removing `play/mybucket/newfile.txt`.
Removing `play/mybucket/otherobject.txt`.
```

_Example: Remove all uploaded incomplete files for an object._

```text
osscli rm --incomplete play/mybucket/myobject.1gig
Removing `play/mybucket/myobject.1gig`.
```

_Example: Remove object and output a message only if the object is created older than 1 day, 2 hours and 30 minutes. Otherwise, the command stays quiet and nothing is printed out._

```text
osscli rm -r --force --older-than 1d2h30m play/mybucket
Removing `play/mybucket/dayOld1.txt`.
Removing `play/mybucket/dayOld2.txt`.
Removing `play/mybucket/dayOld3.txt`.
```

### share

`share` command securely grants upload or download access to object storage. This access is only temporary and it is safe to share with remote users and applications. If you want to grant permanent access, you may look at `osscli policy` command instead.

Generated URL has access credentials encoded in it. Any attempt to tamper the URL will invalidate the access. To understand how this mechanism works, please follow [Pre-Signed URL](http://docs.aws.amazon.com/AmazonS3/latest/dev/ShareObjectPreSignedURL.html) technique.

```text
USAGE:
   osscli share [FLAGS] COMMAND

FLAGS:
  --help, -h                       show help

COMMANDS:
   download   generate URLs for download access
   upload     generate ‘curl’ command to upload objects without requiring access/secret keys
   list       list previously shared objects and folders
```

### share download

`share download` command generates URLs to download objects without requiring access and secret keys. Expiry option sets the maximum validity period \(no more than 7 days\), beyond which the access is revoked automatically.

```text
USAGE:
   osscli share download [FLAGS] TARGET [TARGET...]

FLAGS:
  --recursive, -r               share all objects recursively
  --expire value, -E value      set expiry in NN[h|m|s] (default: "168h")
  --help, -h                    show help
```

_Example: Grant temporary access to an object with 4 hours expiry limit._

```text
osscli share download --expire 4h play/mybucket/myobject.txt
URL: https://play.min.io/mybucket/myobject.txt
Expire: 0 days 4 hours 0 minutes 0 seconds
Share: https://storage-170393591705357.cloud.bigconnect.io/mybucket/myobject.txt?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=Q3AM3UQ867SPQQA43P2F%2F20160408%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20160408T182008Z&X-Amz-Expires=604800&X-Amz-SignedHeaders=host&X-Amz-Signature=1527fc8f21a3a7e39ce3c456907a10b389125047adc552bcd86630b9d459b634
```

### share upload

`share upload` command generates a ‘curl’ command to upload objects without requiring access/secret keys. Expiry option sets the maximum validity period \(no more than 7 days\), beyond which the access is revoked automatically. Content-type option restricts uploads to only certain type of files.

```text
USAGE:
   osscli share upload [FLAGS] TARGET [TARGET...]

FLAGS:
  --recursive, -r                 recursively upload any object matching the prefix
  --expire value, -E value        set expiry in NN[h|m|s] (default: "168h")
  --content-type value, -T value  specify a content-type to allow
  --help, -h                      show help
```

_Example: Generate a `curl` command to enable upload access to `play/mybucket/myotherobject.txt`. User replaces `<FILE>` with the actual filename to upload_

```text
osscli share upload play/mybucket/myotherobject.txt
URL: https://storage-170393591705357.cloud.bigconnect.io/mybucket/myotherobject.txt
Expire: 7 days 0 hours 0 minutes 0 seconds
Share: curl https://storage-170393591705357.cloud.bigconnect.io/mybucket -F x-amz-date=20160408T182356Z -F x-amz-signature=de343934bd0ba38bda0903813b5738f23dde67b4065ea2ec2e4e52f6389e51e1 -F bucket=mybucket -F policy=eyJleHBpcmF0aW9uIjoiMjAxNi0wNC0xNVQxODoyMzo1NS4wMDdaIiwiY29uZGl0aW9ucyI6W1siZXEiLCIkYnVja2V0IiwibXlidWNrZXQiXSxbImVxIiwiJGtleSIsIm15b3RoZXJvYmplY3QudHh0Il0sWyJlcSIsIiR4LWFtei1kYXRlIiwiMjAxNjA0MDhUMTgyMzU2WiJdLFsiZXEiLCIkeC1hbXotYWxnb3JpdGhtIiwiQVdTNC1ITUFDLVNIQTI1NiJdLFsiZXEiLCIkeC1hbXotY3JlZGVudGlhbCIsIlEzQU0zVVE4NjdTUFFRQTQzUDJGLzIwMTYwNDA4L3VzLWVhc3QtMS9zMy9hd3M0X3JlcXVlc3QiXV19 -F x-amz-algorithm=AWS4-HMAC-SHA256 -F x-amz-credential=Q3AM3UQ867SPQQA43P2F/20160408/us-east-1/s3/aws4_request -F key=myotherobject.txt -F file=@<FILE>
```

### share list

`share list` command lists unexpired URLs that were previously shared

```text
USAGE:
   osscli share list COMMAND

COMMAND:
   upload:   list previously shared access to uploads.
   download: list previously shared access to downloads.
```

### mirror

`mirror` command is similar to `rsync`, except it synchronizes contents between filesystems and object storage.

```text
USAGE:
   osscli mirror [FLAGS] SOURCE TARGET

FLAGS:
  --overwrite                        overwrite object(s) on target
  --fake                             perform a fake mirror operation
  --watch, -w                        watch and synchronize changes
  --remove                           remove extraneous object(s) on target
  --region value                     specify region when creating new bucket(s) on target (default: "us-east-1")
  --preserve, -a                     preserve file system attributes and bucket policy rules on target bucket(s)
  --exclude value                    exclude object(s) that match specified object name pattern
  --older-than value                 filter object(s) older than N days (default: 0)
  --newer-than value                 filter object(s) newer than N days (default: 0)
  --storage-class value, --sc value  specify storage class for new object(s) on target
  --encrypt value                    encrypt/decrypt objects (using server-side encryption with server managed keys)
  --encrypt-key value                encrypt/decrypt objects (using server-side encryption with customer provided keys)
  --help, -h                         show help

ENVIRONMENT VARIABLES:
   OSS_ENCRYPT:      list of comma delimited prefixes
   OSS_ENCRYPT_KEY:  list of comma delimited prefix=secret values
```

_Example: Mirror a local directory to 'mybucket' in the play alias:_

```text
osscli mirror localdir/ play/mybucket
localdir/b.txt:  40 B / 40 B  ┃▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓┃  100.00 % 73 B/s 0
```

_Example: Continuously watch for changes on a local directory and mirror the changes to 'mybucket' on the play alias:_

```text
osscli mirror -w localdir play/mybucket
localdir/new.txt:  10 MB / 10 MB  ┃▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓┃  100.00 % 1 MB/s 15s
```

### find

`find` command finds files which match the given set of parameters. It only lists the contents which match the given set of criteria.

```text
USAGE:
  osscli find PATH [FLAGS]

FLAGS:
  --exec value                  spawn an external process for each matching object (see FORMAT)
  --ignore value                exclude objects matching the wildcard pattern
  --name value                  find object names matching wildcard pattern
  --newer value                 match all objects newer than specified time L days, M hours and N minutes
  --older value                 match all objects older than specified time L days, M hours and N minutes
  --path value                  match directory names matching wildcard pattern
  --print value                 print in custom format to STDOUT (see FORMAT)
  --regex value                 match directory and object name with PCRE regex pattern
  --larger value                match all objects larger than specified size in units (see UNITS)
  --smaller value               match all objects smaller than specified size in units (see UNITS)
  --maxdepth value              limit directory navigation to specified depth (default: 0)
  --watch                       monitor a specified path for newly created object(s)
  ...
  ...
  --help, -h                    show help
```

_Example: Find all jpeg images from s3 bucket and copy to OSS "play/bucket" bucket continuously._

```text
osscli find s3/bucket --name "*.jpg" --watch --exec "osscli cp {} play/bucket"
```

### diff

`diff` command computes the differences between the two directories. It only lists the contents which are missing or which differ in size.

It _DOES NOT_ compare the contents, so it is possible that the objects which are of same name and of the same size, but have difference in contents are not detected. This way, it can perform high speed comparison on large volumes or between sites

```text
USAGE:
  osscli diff [FLAGS] FIRST SECOND

FLAGS:
  --config-folder value, -C value  Path to configuration folder. (default: "/root/.mc")
  --quiet, -q                      Disable progress bar display.
  --no-color                       Disable color theme.
  --json                           Enable JSON formatted output.
  --debug                          Enable debug output.
  --insecure                       Disable SSL certificate verification.
  --help, -h                       Show help.

LEGEND:
    < - object is only in source.
    > - object is only in destination.
    ! - newer object is in source.
```

_Example: Compare a local directory and a remote object storage._

```text
osscli diff localdir play/mybucket
‘localdir/notes.txt’ and ‘https://storage-170393591705357.cloud.bigconnect.io/mybucket/notes.txt’ - only in first.
```

#### Option \[--json\]

JSON option enables parseable output in [JSON lines](http://jsonlines.org/) format.

_Example: diff json output._

```text
osscli diff play1/diffbucket play2/diffbucket --json
{"status":"success","first":"","second":"http://127.0.0.1:9001/diffbucket/file1.png","diff":5}
{"status":"success","first":"http://127.0.0.1:9000/diffbucket/file2.png","second":"","diff":4}
{"status":"success","first":"http://127.0.0.1:9000/diffbucket/file3.png","second":"http://127.0.0.1:9001/diffbucket/file3.png","diff":2}
{"status":"success","first":"http://127.0.0.1:9000/diffbucket/file4.png","second":"http://127.0.0.1:9001/diffbucket/file4.png","diff":1}
```

**Diff values in json output**

| Constant | Value | Meaning |
| :--- | :--- | :--- |
| differInNone | 0 | Does not differ |
| differInSize | 1 | Differs in size |
| differInMetadata | 2 | Differs in metadata |
| differInType | 3 | Differs in type exfile/directory |
| differInFirst | 4 | Only in source \(FIRST\) |
| differInSecond | 5 | Only in target \(SECOND\) |

### watch

`watch` provides a convenient way to watch on various types of event notifications on object  
storage and filesystem.

```text
USAGE:
  osscli watch [FLAGS] PATH

FLAGS:
  --events value                   filter specific types of events, defaults to all events (default: "put,delete,get")
  --prefix value                   filter events for a prefix
  --suffix value                   filter events for a suffix
  --recursive                      recursively watch for events
  --help, -h                       show help
```

_Example: Watch for all events on object storage_

```text
osscli watch play/testbucket
[2016-08-18T00:51:29.735Z] 2.7KiB ObjectCreated https://storage-170393591705357.cloud.bigconnect.io/testbucket/CONTRIBUTING.md
[2016-08-18T00:51:29.780Z]  1009B ObjectCreated https://storage-170393591705357.cloud.bigconnect.io/testbucket/MAINTAINERS.md
[2016-08-18T00:51:29.839Z] 6.9KiB ObjectCreated https://storage-170393591705357.cloud.bigconnect.io/testbucket/README.md
```

_Example: Watch for all events on local directory_

```text
Copymc watch ~/Photos
[2016-08-17T17:54:19.565Z] 3.7MiB ObjectCreated /home/play/Downloads/tmp/5467026530_a8611b53f9_o.jpg
[2016-08-17T17:54:19.565Z] 3.7MiB ObjectCreated /home/play/Downloads/tmp/5467026530_a8611b53f9_o.jpg
...
[2016-08-17T17:54:19.565Z] 7.5MiB ObjectCreated /home/play/Downloads/tmp/8771468997_89b762d104_o.jpg
```

### event

`event` provides a convenient way to configure various types of event notifications on a bucket. OSS event notification can be configured to use AMQP, Redis, ElasticSearch, NATS and PostgreSQL services. OSS configuration provides more details on how these services can be configured.

```text
USAGE:
  osscli event COMMAND [COMMAND FLAGS | -h] [ARGUMENTS...]

COMMANDS:
  add     add a new bucket notification
  remove  remove a bucket notification. With '--force' can remove all bucket notifications
  list    list bucket notifications

FLAGS:
  --ignore-existing, -p            ignore if event already exists
  --help, -h                       show help
```

_Example: List all configured bucket notifications_

```text
osscli event list play/andoria
MyTopic        arn:minio:sns:us-east-1:1:TestTopic    s3:ObjectCreated:*,s3:ObjectRemoved:*   suffix:.jpg
```

_Example: Add a new 'sqs' notification resource only to notify on ObjectCreated event_

```text
osscli event add play/andoria arn:minio:sqs:us-east-1:1:your-queue --event put
```

_Example: Add a new 'sqs' notification resource with filters_

Add `prefix` and `suffix` filtering rules for `sqs` notification resource.

```text
osscli event add play/andoria arn:minio:sqs:us-east-1:1:your-queue --prefix photos/ --suffix .jpg
```

_Example: Remove a 'sqs' notification resource_

```text
osscli event remove play/andoria arn:minio:sqs:us-east-1:1:your-queue
```

### policy

Manage anonymous bucket policies to a bucket and its contents

```text
USAGE:
  osscli policy [FLAGS] set PERMISSION TARGET
  osscli policy [FLAGS] set-json FILE TARGET
  osscli policy [FLAGS] get TARGET
  osscli policy [FLAGS] get-json TARGET
  osscli policy [FLAGS] list TARGET

PERMISSION:
  Allowed policies are: [none, download, upload, public].

FILE:
  A valid S3 policy JSON filepath.

FLAGS:
  --help, -h                       show help
```

_Example: Show current anonymous bucket policy_

Show current anonymous bucket policy for `mybucket/myphotos/2020/` sub-directory

```text
osscli policy get play/mybucket/myphotos/2020/
Access permission for ‘play/mybucket/myphotos/2020/’ is ‘none’
```

_Example : Set anonymous bucket policy to download only_

Set anonymous bucket policy for `mybucket/myphotos/2020/` sub-directory and its objects to `download` only. Now, objects under the sub-directory are publicly accessible. e.g `mybucket/myphotos/2020/yourobjectname`is available at `https://storage-170393591705357.cloud.bigconnect.io/mybucket/myphotos/2020/yourobjectname`

```text
osscli policy set download play/mybucket/myphotos/2020/
Access permission for ‘play/mybucket/myphotos/2020/’ is set to 'download'
```

_Example : Set anonymous bucket policy from a JSON file_

Configure bucket policy for `mybucket` with a policy JSON file.

```text
osscli policy set-json /tmp/policy.json play/mybucket
Access permission for `play/mybucket` is set from `/tmp/policy.json`
```

_Example : Remove current anonymous bucket policy_

Remove any bucket policy for _mybucket/myphotos/2020/_ sub-directory.

```text
osscli policy set none play/mybucket/myphotos/2020/
Access permission for ‘play/mybucket/myphotos/2020/’ is set to 'none'
```

### tag

`tag` command provides a convenient way to set, remove, and list bucket/object tags. Tags are defined as key-value pairs.

```text
USAGE:
  osscli tag COMMAND [COMMAND FLAGS | -h] [ARGUMENTS...]

COMMANDS:
  list     list tags of a bucket or an object
  remove   remove tags assigned to a bucket or an object
  set      set tags for a bucket or an object

FLAGS:
  --help, -h                    show help
  --json                        enable JSON formatted output
  --debug                       enable debug output
```

_Example : List tags assigned to an object_

List tags for `testobject` in `testbucket` in alias `s3`

```text
osscli tag list s3/testbucket/testobject
Name                :    testobject
editable            :    only-by-owner-and-authenticated
confidentiality     :    open-to-authenticated-only
```

_Example : Set tags for an object_

Set tags for `testobject` in `testbucket` in alias `s3`

```text
osscli tag set s3/testbucket/testobject "key1=value1&key2=value2&key3=value3"
Tags set for s3/testbucket/testobject.
```

_Example : Remove tags assigned to an object_

Remove tags assigned to `testobject` in `testbucket` in alias `s3`

```text
osscli tag remove s3/testbucket/testobject
Tags removed for s3/testbucket/testobject.
```

