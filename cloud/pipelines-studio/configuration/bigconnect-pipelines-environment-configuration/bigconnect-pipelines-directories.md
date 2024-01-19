# BigConnect Pipelines Directories

BigConnect Pipelines includes environment variables that define the directories used to store configuration, data, log, and resource files.

The SDC\_DIST environment variable defines the BigConnect Pipelines runtime directory. The runtime directory is the base BigConnect Pipelines directory that stores the executables and related files. This environment variable is set during installation.

When you start BigConnect Pipelines manually, the default values of the remaining directory variables are relative to the `$SDC_DIST` runtime directory. When you start BigConnect Pipelines as a service, the default values of the remaining directory variables are absolute paths that are outside of the `$SDC_DIST` runtime directory.

Modify environment variables using the method required by your installation type.

**Note**: BigConnect Pipelines does not recommend using NFS or NAS to store BigConnect Pipelines files.

You can configure the following environment variables that define directories:

| Environment Variable | Description                                                                                                                                                                                                                                                                                                                                           |
| -------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| SDC\_CONF            | <p>Defines the configuration directory for the BigConnect Pipelines configuration file, <code>sdc.properties</code>, and related realm properties files and keystore files. Also includes the logj4 properties file.</p><p>Default values:</p><ul><li>Manual start: <code>$SDC_DIST/etc</code></li><li>Service start: <code>/etc/sdc</code></li></ul> |
| SDC\_DATA            | <p>Defines the data directory for pipeline configuration and run details.</p><p>Default values:</p><ul><li>Manual start: <code>$SDC_DIST/data</code></li><li>Service start: <code>/var/lib/sdc</code></li></ul>                                                                                                                                       |
| SDC\_LOG             | <p>Defines the log directory.</p><p>Default values:</p><ul><li>Manual start: <code>$SDC_DIST/log</code></li><li>Service start: <code>/var/log/sdc</code></li></ul>                                                                                                                                                                                    |
| SDC\_RESOURCES       | <p>Defines the directory for runtime resource files.</p><p>Default values:</p><ul><li>Manual start: <code>$SDC_DIST/resources</code></li><li>Service start: <code>/var/lib/sdc-resources</code></li></ul>                                                                                                                                             |
