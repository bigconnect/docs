# Java Configuration Options

You define Java configuration options used by BigConnect Pipelines in environment variables.

For a tarball or RPM installation, define Java configuration options in the following environment variables:

* SDC\_JAVA\_OPTS - Includes configuration options for Java.
* SDC\_JAVA8\_OPTS - Includes configuration options specific to Java 8.

BigConnect Pipelines loads the value of the version-specific environment variable and adds it to the SDC\_JAVA\_OPTS environment variable.

When defining Java configuration options, avoid defining duplicate options. If you do define duplicates, the last option passed to the JVM usually takes precedence.

For a Cloudera Manager installation, define Java configuration options by configuring the BigConnect Pipelines service through Cloudera Manager.

**Note**: To modify Java configuration options for the BigConnect Pipelinescommand line interface, use the SDC\_CLI\_JAVA\_OPTS environment variable. For more information, see Java Configuration Options for the Cli Command.
