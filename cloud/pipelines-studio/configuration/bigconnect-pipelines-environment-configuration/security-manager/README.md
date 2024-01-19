# Security Manager

BigConnect Pipelines includes a Java Security Manager that is enabled by default. For enhanced security, you can enable the BigConnect Pipelines Security Manager which prevents stages from accessing files in protected BigConnect Pipelines directories.

BigConnect Pipelines can use one of the following security managers:

**Java Security Manager**

By default, BigConnect Pipelines uses the Java Security Manager. The Java Security Manager restricts the runtime permissions of user libraries. This allows administrators to control user libraries actions on production systems. For example, by default, user libraries cannot call out to network resources and potentially cause denial-of-service (DDoS) attacks.

The security policy is defined in the `$SDC_CONF/sdc-security.policy` file. The file syntax is java standard.

**BigConnect Pipelines Security Manager**

For enhanced security, enable the BigConnect Pipelines Security Manager. The BigConnect Pipelines Security Manager prevents stages from accessing files in protected BigConnect Pipelines directories, regardless of how the `sdc-security.policy` file is defined.\
\
To enable the BigConnect Pipelines Security Manager, uncomment the `security_manager.sdc_manager.enable` property in the BigConnect Pipelines configuration file, `$SDC_CONF/sdc.properties`.\
\
**Note**: If you use an older JVM version, the BigConnect Pipelines Security Manager might encounter some JVM known issues.

If needed, you can configure BigConnect Pipelines to use neither security manager by setting the SDC\_SECURITY\_MANAGER\_ENABLED environment variable to false.

Modify environment variables using the method required by your installation type.
