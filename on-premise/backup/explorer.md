# Explorer

## Backup

To make the backup, first stop all running services.

1. Export Accumulo tables:

```text
/opt/bdl/bin/accumulo shell -u root

clonetable bc_v bc_v_exp
clonetable bc_e bc_e_exp
clonetable bc_d bc_d_exp
clonetable bc_m bc_m_exp
clonetable bc_extdata bc_extdata_exp
clonetable bc_systemNotifications bc_systemNotifications_exp
clonetable bc_userNotifications bc_userNotifications_exp

offline bc_v_exp
offline bc_e_exp
offline bc_d_exp
offline bc_m_exp
offline bc_extdata_exp
offline bc_systemNotifications_exp
offline bc_userNotifications_exp

exporttable -t bc_v_exp /tmp/bc_v_exp
exporttable -t bc_e_exp /tmp/bc_e_exp
exporttable -t bc_d_exp /tmp/bc_d_exp
exporttable -t bc_m_exp /tmp/bc_m_exp
exporttable -t bc_extdata_exp /tmp/bc_extdata_exp
exporttable -t bc_systemNotifications_exp /tmp/bc_systemNotifications_exp
exporttable -t bc_userNotifications_exp /tmp/bc_userNotifications_exp
```

2. Make a note of the user's authorizations:

```text
getauths
```

3. Run the export:

```text
/opt/bdl/bin/hadoop distcp -f /tmp/bc_v_exp/distcp.txt /tmp/bc_v_exp/files
/opt/bdl/bin/hadoop distcp -f /tmp/bc_e_exp/distcp.txt /tmp/bc_e_exp/files
/opt/bdl/bin/hadoop distcp -f /tmp/bc_d_exp/distcp.txt /tmp/bc_d_exp/files
/opt/bdl/bin/hadoop distcp -f /tmp/bc_m_exp/distcp.txt /tmp/bc_m_exp/files
/opt/bdl/bin/hadoop distcp -f /tmp/bc_extdata_exp/distcp.txt /tmp/bc_extdata_exp/files
/opt/bdl/bin/hadoop distcp -f /tmp/bc_systemNotifications_exp/distcp.txt /tmp/bc_systemNotifications_exp/files
/opt/bdl/bin/hadoop distcp -f /tmp/bc_userNotifications_exp/distcp.txt /tmp/bc_userNotifications_exp/files

/opt/bdl/bin/alluxio fs copyToLocal /tmp/bc_v_exp/files /tmp/backup/bc_v_exp/files
/opt/bdl/bin/alluxio fs copyToLocal /tmp/bc_e_exp/files /tmp/backup/bc_e_exp/files
/opt/bdl/bin/alluxio fs copyToLocal /tmp/bc_d_exp/files /tmp/backup/bc_d_exp/files
/opt/bdl/bin/alluxio fs copyToLocal /tmp/bc_m_exp/files /tmp/backup/bc_m_exp/files
/opt/bdl/bin/alluxio fs copyToLocal /tmp/bc_extdata_exp/files /tmp/backup/bc_extdata_exp/files
/opt/bdl/bin/alluxio fs copyToLocal /tmp/bc_systemNotifications_exp/files /tmp/backup/bc_systemNotifications_exp/files
/opt/bdl/bin/alluxio fs copyToLocal /tmp/bc_userNotifications_exp/files /tmp/backup/bc_userNotifications_exp/files

tar cvfz /tmp/backup.tgz /tmp/backup/
```

4. Backup the **/opt/bdl/data/elastic** folder

## Restore

1. Restore the **/opt/bdl/data/elastic** folder

2. Unpack the backup.tgz file and copy it's contents in HDFS:

```text
tar zxvf backup.tgz
/opt/bdl/bin/alluxio fs mkdir /tmp/
/opt/bdl/bin/alluxio fs copyFromLocal ./tmp/backup /tmp/
```

3. Import Accumulo tables:

```text
importtable bc_v /tmp/bc_v_exp/files
importtable bc_e /tmp/bc_e_exp/files
importtable bc_d /tmp/bc_d_exp/files
importtable bc_m /tmp/bc_m_exp/files
importtable bc_extdata /tmp/bc_extdata_exp/files
importtable bc_systemNotifications /tmp/bc_systemNotifications_exp/files
importtable bc_userNotifications /tmp/bc_userNotifications_exp/files
```

4. Set the Accumulo authorizations from Backup - Step 2 :

```text
setauths -c
setauths -s <authorizations>
```

5. Start services

