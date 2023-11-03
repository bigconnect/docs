# Database Support

Using H2, MySQL, PostgreSQL, Oracle, or DB2 with JBatch

You can configure **JBatch** any of the following RDBMS engines in the same way that you would configure it to use **Derby**:

* H2
* MySQL
* PostgreSQL
* Oracle
* IBM DB2

### Configuration

On the administration console:

1. Create a Connection Pool:
   1. Navigate to _Resources_ → _JDBC_ → _JDBC Connection Pools_ and click on _New…​_
   2. Give it a name in the _Pool Name_ field, select the resource type from the _Resource Type_ drop-down, and choose the _Database Driver Vendor_ as either **H2**, **DB2**, **MySQL**, **Oracle**, or **PostgreSQL** from the _Database Driver Vendor_ menu.
   3. Set any further configuration options on the next page.
   4. Click _Finish_
2. Create a JDBC Resource:
   1. Navigate to _Resources_ → _JDBC_ → _JDBC Resources_ and click on _New…​_
   2. Give it a name in the _JNDI Name_ field, and select the data source you just created from the _Pool Name_ drop-down.
   3. Add any additional properties and select the targets for it to be enabled on.
   4. Click _OK_.
3. Navigate to the Batch configuration page of the instance or cluster:
4. Click on the instance or cluster to move to its configuration page.
5. Select the _Batch_ tab, and the _Configuration_ sub-tab should load.
6. Select the new data source from the _Data Source Lookup Name_ drop-down menu.
7. Save the changes.

### Usage Restrictions

**JBatch** will not create internally more than one set of JBatch tables per schema. So in your schema if there exists a set of JBatch tables with prefixes and suffixes in the table names and then specify in the Glassfish JBatch configuration for the same schema above that you wish to use JBatch tables under a different name (for example no table prefix and suffix) then during the JBatch initialization phase, JBatch will attempt to create these tables since they do not exist.

However since the table constraint names already exist for the existing JBatch tables in the same schema, table creation will fail. One can of course run a _SQL_ script to create the relevant JBatch schema objects under different names.

### MySQL Specifics

For MySQL database use, it is recommended the following additional property be set:

| Name                 | Value         | Description                                                                                               |
| -------------------- | ------------- | --------------------------------------------------------------------------------------------------------- |
| zeroDateTimeBehavior | convertToNull | Action for DATETIME values that are composed entirely of zeros (used by MySQL to represent invalid dates) |
