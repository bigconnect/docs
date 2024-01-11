# Remote Debugging

You can enable remote debugging to debug a BigConnect Pipelines instance running on a remote machine.

Enable remote debugging based on your installation:\
\
**Tarball or RPM installation**

Define debugging options in the SDC\_JAVA\_OPTS environment variable.

Add the following debugging options to the environment variable, where `port_number` is an open port number on the remote machine running BigConnect Pipelines:

```
-Xdebug -Xrunjdwp:server=y,transport=dt_socket,address=<port_number>,suspend=n
```

For example, to debug BigConnect Pipelines on a remote machine using port number 2005, define SDC\_JAVA\_OPTS as follows:

```
export SDC_JAVA_OPTS="${SDC_JAVA_OPTS} -Xdebug -Xrunjdwp:server=y,transport=dt_socket,address=2005,suspend=n"
```

Modify environment variables using the method required by your installation type.



**Cloudera Manager installation**\
\
Define the debugging options in the Java Options property for the BigConnect Pipelines service in Cloudera Manager.\
\
For example, to debug BigConnect Pipelines on a remote machine using port number 2005, add the debugging options to the property as follows:

```
-Xdebug -Xrunjdwp:server=y,transport=dt_socket,address=2005,suspend=n
```
