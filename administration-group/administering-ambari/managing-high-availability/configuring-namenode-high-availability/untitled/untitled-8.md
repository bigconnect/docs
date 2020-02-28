# Prepare the Ambari server host for NameNode rollback

To prepare for the NameNode rollback task:  
  
**Steps**

1. Log in to the Ambari server host. 
2. Set the following environment variables:  **export AMBARI\_USER=AMBARI\_USERNAME** Substitute the value of the administrative user for Ambari Web. The default value is `admin`.  **export AMBARI\_PW=AMBARI\_PASSWORD** Substitute the value of the administrative password for Ambari Web. The default value is `admin`.  **export AMBARI\_PORT=AMBARI\_PORT** Substitute the Ambari Web port. The default value is `8080`.  **export AMBARI\_PROTO=AMBARI\_PROTOCOL** Substitute the value of the protocol for connecting to Ambari Web. Options are http or https. The default value is `http`.  **export CLUSTER\_NAME=CLUSTER\_NAME** Substitute the name of your cluster, which you set during installation: for example, `mycluster`.  **export NAMENODE\_HOSTNAME=NN\_HOSTNAME** Substitute the FQDN of the host for the non-HA NameNode: for example, `nn01.mycompany.com`.  **export ADDITIONAL\_NAMENODE\_HOSTNAME=ANN\_HOSTNAME** Substitute the FQDN of the host for the additional NameNode in your HA setup.  **export SECONDARY\_NAMENODE\_HOSTNAME=SNN\_HOSTNAME** Substitute the FQDN of the host for the standby NameNode for the non-HA setup.  **export JOURNALNODE1\_HOSTNAME=JOUR1\_HOSTNAME** Substitute the FQDN of the host for the first Journal Node.  **export JOURNALNODE2\_HOSTNAME=JOUR2\_HOSTNAME** Substitute the FQDN of the host for the second Journal Node.  **export JOURNALNODE3\_HOSTNAME=JOUR3\_HOSTNAME** Substitute the FQDN of the host for the third Journal Node. 
3. Double check that these environment variables are set correctly.

**After you finish**  
Restore the HBase configuration.

