# Limits

#### Browser Access

| Item | Specification |
| :--- | :--- |
| Web browser upload size limit | 5 TiB |

#### Limits of API

| Item | Specification |
| :--- | :--- |
| Maximum number of buckets | no-limit |
| Maximum number of objects per bucket | no-limit |
| Maximum object size | 5 TiB |
| Minimum object size | 0 B |
| Maximum object size per PUT operation | 5 TiB |
| Maximum number of parts per upload | 10,000 |
| Part size | 5 MiB to 5 GiB. Last part can be 0 B to 5 GiB |
| Maximum number of parts returned per list parts request | 10000 |
| Maximum number of objects returned per list objects request | 10000 |
| Maximum number of multipart uploads returned per list multipart uploads request | 1000 |

### Object name restrictions

Object names that contain characters `^*|\/&";` are unsupported on Windows and other file systems which do not support filenames with these characters. Note that this list is not exhaustive, and depends on the maintainers of the filesystem itself.

