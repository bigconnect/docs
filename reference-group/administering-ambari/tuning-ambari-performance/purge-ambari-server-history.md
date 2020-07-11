# Purge Ambari server history

To reduce performance degradation, use an Ambari server CLI command to automate removal of historical records from the Ambari server database.

After months of operating a larger cluster, an Ambari server may begin to accrue a large amount of historical data in its database. Data accrual can cause UI performance degradation.The db-purge-history command takes two arguments, the name of the cluster, and the date of the earliest record to purge. The following example steps demonstrate purging history records created before August 1st, 2017 for the cluster named \[PROD\].

1.Stop the Ambari Server

2. Run `db-purge-history`

```text
# ambari-server db-purge-history --cluster-name [PROD] --from-date 2017-08-01
Using python  /usr/bin/python
Purge database history...
Ambari Server configured for Embedded Postgres. Confirm you have made a backup of the Ambari Server database [y/n] y
Ambari server is using db type Embedded Postgres. Cleanable database entries older than 2017-08-01 will be purged. Proceed [y/n] y
Purging historical data from the database ...
Purging historical data completed. Check the ambari-server.log for details.
Ambari Server 'db-purge-history' completed successfully.
```

3. Start the Ambari Server

The db-purge-history command analyzes tables in the Ambari Server database and removes those rows that can be deleted which have a --create-date after the --from-date specified when the command runs. In this example, the rows deleted would be:

* AlertCurrent
* AlertNotice
* ExecutionCommand
* HostRoleCommand
* Request
* RequestOperationLevel
* RequestResourceFilter
* RoleSuccessCriteria
* Stage
* TopologyHostRequest
* TopologyHostTask
* TopologyLogicalTask

