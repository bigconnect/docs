# BigConnect Pipelines Configuration

You can edit the BigConnect Pipelines configuration file, `$SDC_CONF/sdc.properties`, to configure properties such as the host name and port number and account information for email alerts.

You can protect sensitive data in BigConnect Pipelines configuration files by storing the data in an external location and then using functions provided with the StreamSets expression language to retrieve the data. You can also reference information in an environment variable.

You can define runtime properties in the BigConnect Pipelines configuration file or in a separate file. For more information, see Using Runtime Properties.\
\
**Important:** When you use Cloudera Manager to install BigConnect Pipelines, you must use Cloudera Manager to configure BigConnect Pipelines properties and environment. Manual changes to the BigConnect Pipelines properties or environment files are not recognized by Cloudera Manager. For more information, see Configuring BigConnect Pipelines with Cloudera Manager.
