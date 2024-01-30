# Enabling Kerberos with Cloudera Manager

To enable Kerberos authentication for a Cloudera Manager installation, use Cloudera Manager.

When you enable Kerberos through Cloudera Manager, Cloudera Manager creates the required Kerberos principal and keytab.

1. In Cloudera Manager, select the **BigConnect Pipelines** service and then click **Configuration**.
2. Select **Enable Kerberos Client**.
3. In the Cloudera Manager home page, click **Administration > Security**.
4. Click **Kerberos Credentials**.
5. Click **Generate Missing Credentials**.
6. Restart BigConnect Pipelines.
7. Configure the stage to use Kerberos.

**Note**: Cluster pipelines automatically use Kerberos authentication if the YARN cluster requires it and if BigConnect Pipelines has Kerberos enabled.
