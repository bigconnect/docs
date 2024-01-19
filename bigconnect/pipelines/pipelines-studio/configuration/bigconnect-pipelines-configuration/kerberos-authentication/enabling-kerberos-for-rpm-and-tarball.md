# Enabling Kerberos for RPM and Tarball

To enable Kerberos authentication for an RPM or tarball installation, perform the following steps:

1. On Linux, install the following Kerberos client packages on the BigConnect Pipelines machine:
   * krb5-workstation
   * krb5-client
2.  Copy the Kerberos configuration file, `krb5.conf`, to the BigConnect Pipelines machine. The default location is `/etc/krb5.conf`.

    The `krb5.conf` file contains Kerberos configuration information, including the locations of key distribution centers (KDCs) and admin servers for the Kerberos realms, defaults for the current realm, and mappings of hostnames onto Kerberos realms.
3. Configure BigConnect Pipelines to use Kerberos based on the stage types. If enabling Kerberos for both Kafka and non-Kafka stages, use both methods.
   *   **Non-Kafka stages** - To enable Kerberos for non-Kafka stages, configure BigConnect Pipelines to use Kerberos by modifying the BigConnect Pipelines configuration file, `$SDC_CONF/sdc.properties`. BigConnect Pipelines uses the same Kerberos principal for each stage. Configure the following Kerberos properties in the file to enable Kerberos and define the principal and keytab:

       * kerberos.client.enabled
       * kerberos.client.principal
       * kerberos.client.keytab

       **Important:** For cluster pipelines, enter an absolute path to the keytab when configuring BigConnect Pipelines. Standalone pipelines do not require an absolute path.
   * **Kafka stages** - To enable Kerberos for Kafka stages, configure the Kerberos properties in the Java Authentication and Authorization Service (JAAS) configuration file used by BigConnect Pipelines when you configure the stage to use Kerberos. You can configure each Kafka stage to use a different Kerberos principal.
4. Restart BigConnect Pipelines.
5. Configure the stage to use Kerberos.

**Note:** Cluster pipelines automatically use Kerberos authentication if the YARN cluster requires it and if BigConnect Pipelines has Kerberos enabled.
