# Protected Directories

When the BigConnect Pipelines Security Manager is enabled, the following BigConnect Pipelines directories are protected directories:

* `$SDC_CONF` - Stages cannot access files in the configuration directory.
* `$SDC_DATA` - Stages cannot access files in the data directory.
* `$SDC_RESOURCES` - Stages can read files in the resources directory, but cannot write to files in the directory.

If needed, you can allow stages to access specific files in these protected directories by modifying BigConnect Pipelines Security Manager exception properties in the BigConnect Pipelines configuration file, $SDC\_CONF/sdc.properties. However, use caution when configuring exceptions to these protected directories.

You can configure exceptions to protected directories as follows:\
\
**Exceptions for all stage libraries**\
\
To allow all stage libraries access to files in protected directories, modify the `security_manager.sdc_dirs.exceptions` property to define files that can be accessed.\
\
**Exceptions for specific stage libraries**\
\
To allow a specific stage library access to files in protected directories, add the following property and then define the files that the stage library can access:

```
security_manager.sdc_dirs.exceptions.<stage_library_name>=<file_path>
```

For example, the default BigConnect Pipelines configuration file includes an exception for the Java keystore credential store stage library defined as follows:

```
security_manager.sdc_dirs.exceptions.lib.streamsets-datacollector-jks-credentialstore-lib=$SDC_CONF/jks-credentialStore.pkcs12
```

When you configure a Security Manager exception property, use the appropriate directory environment variable in the file path: `$SDC_CONF`, `$SDC_DATA`, or `$SDC_RESOURCES`. You can enter multiple file paths separated by commas.
