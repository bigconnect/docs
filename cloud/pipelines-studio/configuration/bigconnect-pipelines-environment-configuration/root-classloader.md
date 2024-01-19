# Root Classloader

You can edit the SDC\_ROOT\_CLASSPATH environment variable to define the path to JAR files to be added to the BigConnect Pipelines root classloader.

Use the variable for components that must be in the root classloader, such as Snappy. Default is `$SDC_DIST/root-lib/'*'`.

Modify environment variables using the method required by your installation type.
