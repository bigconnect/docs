# CONNECT error class

SQLSTATE: none assigned

Generic Spark Connect error.

This error class has the following derived error classes:

### INTERCEPTOR\_CTOR\_MISSING

Cannot instantiate GRPC interceptor because `<cls>` is missing a default constructor without arguments.

### INTERCEPTOR\_RUNTIME\_ERROR

Error instantiating GRPC interceptor: `<msg>`

### PLUGIN\_CTOR\_MISSING

Cannot instantiate Spark Connect plugin because `<cls>` is missing a default constructor without arguments.

### PLUGIN\_RUNTIME\_ERROR

Error instantiating Spark Connect plugin: `<msg>`

### SESSION\_NOT\_SAME

Both Datasets must belong to the same SparkSession.
