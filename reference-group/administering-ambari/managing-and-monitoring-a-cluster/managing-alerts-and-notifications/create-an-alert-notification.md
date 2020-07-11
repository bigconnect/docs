# Create an alert notification

Use **Alerts &gt; Actions &gt; Manage Notifications** to create or edit alert notifications.

**Steps**

1. In **Ambari Web**, browse to browse to **Alerts &gt; Actions &gt; Manage Notifications**. 
2. In **Manage Alert Notifications**, click **+**. 
3. In **Create Alert Notification**, complete the fields.  


   a. In **Name**, enter a name for the notification.  
   b.In **Groups**, click **All** or **Custom** to assign the notification to every or set of groups that you specify.  
   c. In **Description**, type a phrase that describes the notification.  
   d. In **Method**, click **EMAIL, SNMP** \(for MIB-based\) or **Custom SNMP** as the method by which Ambari server handles delivery of this notification.  

4. Complete the fields for the notification method you selected. For **email notification** - provide information about your SMTP infrastructure, such as SMTP server, port, to and from addresses, and whether authentication is required to relay messages through the server. You can add custom properties to the SMTP configuration based on Javamail SMTP options.  **Email To** A comma-separated list of one or more email addresses to which to send the alert email  **SMTP Server** The FQDN or IP address of the SMTP server to use to relay the alert email  **SMTP Port** The SMTP port on the SMTP server  **Email From** A single email address to be the originator of the alert email  **Use Authentication** Determine whether your SMTP server requires authentication before it can relay messages. Be sure to also provide the username and password credentials  For **MIB-based SNMP notification** - provide the version, community, host, and port to which the SNMP trap should be sent.  **Version** SNMPv1 or SNMPv2c, depending on the network environment  **Hosts** A comma-separated list of one or more host FQDNs to which to send the trap  **Port** The port on which a process is listening for SNMP traps  For **SNMP notifications** - Ambari uses a MIB, a text file manifest of alert definitions, to transfer alert information from cluster operations to the alerting infrastructure. A MIB summarizes how object IDs map to objects or attributes. For example, MIB file content looks like this: ![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/managing-and-monitoring-ambari/how-to/images/amb_ex_mib_file.png) You can find the MIB file for your cluster on the Ambari Server host, at `/var/lib/ambari-server/resources/APACHE-AMBARI-MIB.txt.`  For **Custom SNMP notification** - provide the version, community, host, and port to which the SNMP trap should be sent. Also, the OID parameter must be configured properly for SNMP trap context. If no custom, enterprise-specific OID is used, you should use the following:  **Version** SNMPv1 or SNMPv2c, depending on the network environment  **OID** 1.3.6.1.4.1.18060.16.1.1  **Hosts** A comma-separated list of one or more host FQDNs to which to send the trap  **Port** The port on which a process is listening for SNMP traps 
5. **In Create Alert Notification**, click **Save**.

