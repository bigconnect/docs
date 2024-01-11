# BigConnect Pipelines Environment Configuration

BigConnect Pipelines includes several environment variables that you can modify to customize the following areas:

* BigConnect Pipelines directories
* User and group used to start BigConnect Pipelines as a service
* Java configuration options
* Security Manager that restricts the runtime permissions of user libraries
* Path to JAR files to be added to the root classloader
* Heap dump creation and file location

**Note**: BigConnect Pipelines also includes a SPARK\_KAFKA\_VERSION environment variable that should not be modified. This variable is used only when you run cluster streaming mode pipelines on a Cloudera CDH cluster. For more information, see Kafka Cluster Requirements.
