# Data Definition Language (DDL)

### CREATE TABLE

The command creates a new Ignite cache and defines a SQL table on top of it. The underlying cache stores the data in the form of key-value pairs while the table allows processing the data with SQL queries.

The table will reside in the Schema specified in the connection parameters. If no schema is specified, the `PUBLIC` will be used as a default.

The `CREATE TABLE` command is synchronous. Moreover, it blocks the execution of other DDL commands that are issued before the `CREATE TABLE` command has finished execution. The execution of DML commands is not affected and can be performed in parallel.

```
CREATE TABLE [IF NOT EXISTS] tableName (tableColumn [, tableColumn]...
[, PRIMARY KEY (columnName [,columnName]...)])
[WITH "paramName=paramValue [,paramName=paramValue]..."]

tableColumn := columnName columnType [DEFAULT defaultValue] [PRIMARY KEY]
```

Parameters:

* `tableName` - name of the table.
* `tableColumn` - name and type of a column to be created in the new table.
* `columnName` - name of a previously defined column.
* `DEFAULT` - specifies a default value for the column. Only constant values are accepted.
* `IF NOT EXISTS` - create the table only if a table with the same name does not exist.
* `PRIMARY KEY` - specifies a primary key for the table that can consist of a single column or multiple columns.
* `WITH` - accepts additional parameters not defined by ANSI-99 SQL:
  * `TEMPLATE=<cache’s template name>` - case-sensitive​ name of a cache template. A template is an instance of the `CacheConfiguration` class registered by calling `Ignite.addCacheConfiguration()`. Use predefined `TEMPLATE=PARTITIONED` or `TEMPLATE=REPLICATED` templates to create the cache with the corresponding replication mode. The rest of the parameters will be those that are defined in the `CacheConfiguration` object. By default, `TEMPLATE=PARTITIONED` is used if the template is not specified explicitly.
  * `BACKUPS=<number of backups>` - sets the number of partition backups. If neither this nor the `TEMPLATE` parameter is set, then the cache is created with `0` backup copies.
  * `ATOMICITY=<ATOMIC | TRANSACTIONAL | TRANSACTIONAL_SNAPSHOT>` - sets atomicity mode for the underlying cache. If neither this nor the `TEMPLATE` parameter is set, then the cache is created with the `ATOMIC` mode enabled. If `TRANSACTIONAL_SNAPSHOT` is specified, the table will support transactions.
  * `WRITE_SYNCHRONIZATION_MODE=<PRIMARY_SYNC | FULL_SYNC | FULL_ASYNC>` - sets the write synchronization mode for the underlying cache. If neither this nor the `TEMPLATE` parameter is set, then the cache is created with `FULL_SYNC` mode enabled.
  * `CACHE_GROUP=<group name>` - specifies the group name the underlying cache belongs to.
  * `AFFINITY_KEY=<affinity key column name>` - specifies an affinity key name which is a column of the `PRIMARY KEY` constraint.
  * `CACHE_NAME=<custom name of the new cache>` - the name of the underlying cache created by the command, or the `SQL_{SCHEMA_NAME}_{TABLE}` format will be used if the parameter not specified.
  * `DATA_REGION=<existing data region name>` - name of the data region where table entries should be stored. By default, Ignite stores all the data in a default region.
  * `PARALLELISM=<number of SQL execution threads>` - SQL queries are executed by a single thread on each node by default, but certain scenarios can benefit from multi-threaded execution.
  * `KEY_TYPE=<custom name of the key type>` - sets the name of the custom key type that is used from the key-value APIs in Ignite. The name should correspond to a Java, .NET, or C++ class, or it can be a random one if BinaryObjects is used instead of a custom class. The number of fields and their types in the custom key type has to correspond to the `PRIMARY KEY`.&#x20;
  * `VALUE_TYPE=<custom name of the value type of the new cache>` - sets the name of a custom value type that is used from the key-value and other non-SQL APIs in Ignite. The name should correspond to a Java, .NET, or C++ class, or it can be a random one if BinaryObjects is used instead of a custom class. The value type should include all the columns defined in the CREATE TABLE command except for those listed in the `PRIMARY KEY` constraint. .
  * `WRAP_KEY=<true | false>` - this flag controls whether a _single column_ `PRIMARY KEY` should be wrapped in the BinaryObjects format or not. By default, this flag is set to false. This flag does not have any effect on the `PRIMARY KEY` with multiple columns; it always gets wrapped regardless of the value of the parameter.
  * `WRAP_VALUE=<true | false>` - this flag controls whether a single column value of a primitive type should be wrapped in the BinaryObjects format or not. By default, this flag is set to true. This flag does not have any effect on the value with multiple columns; it always gets wrapped regardless of the value of the parameter. Set this parameter to false if you have a single column value and do not plan to add additional columns to the table. Note that once the parameter is set to false, you can’t use the `ALTER TABLE ADD COLUMN` command for this specific table.

Read more about the database architecture on the SQL Introduction page.

#### Define Primary Key

The example below shows how to create a table with `PRIMARY KEY` specified in the column definition and override cache related parameters. A new distributed cache `SQL_PUBLIC_PERSON` will be created (the `SQL_{SCHEMA_NAME}_{TABLE}` format is used for naming) which stores objects of the `Person` type that corresponds to a specific Java, .NET, C++ class or BinaryObject.

The distributed cache related parameters are passed in the `WITH` clause of the statement. If the `WITH` clause is omitted, then the cache will be created with default parameters set in the `CacheConfiguration` object.

```
CREATE TABLE Person (
  id int PRIMARY KEY,
  city_id int,
  name varchar,
  age int,
  company varchar
) WITH "atomicity=transactional,cachegroup=somegroup";
```

#### Use non-SQL API

If you wish to access the table data by the key-value or other non-SQL API, then you might be need to set the `CACHE_NAME` and `KEY_TYPE`, `VALUE_TYPE` parameters corresponding to your business model objects to make non-SQL APIs usage more convenient.

* Use the `CACHE_NAME` parameter to override the default name with the following format `SQL_{SCHEMA_NAME}_{TABLE}`.
* By default, the command also creates two new binary types - for the key and value respectively. Ignite in turn generates the names of the types randomly including a UUID string which complicates the usage of these types from a non-SQL API.

The example below shows how to create a table `PERSON` and the underlying cache with the same name. The cache will store objects of the `Person` type with explicitly defined the key type `PersonKey` and value type `PersonValue`. The `PRIMARY KEY` columns will be used as the object’s key, the rest of the columns will belong to the value.

```
CREATE TABLE IF NOT EXISTS Person (
  id int,
  city_id int,
  name varchar,
  age int,
  company varchar,
  PRIMARY KEY (id, city_id)
) WITH "template=partitioned,backups=1,affinity_key=city_id,CACHE_NAME=Person,KEY_TYPE=PersonKey,VALUE_TYPE=PersonValue";
```

#### Use non-Upper Case Columns

Ignite parses all unquoted identifiers, names of a table columns and converts them to uppercase during the `CREATE TABLE` command execution which, in turn, makes the command with explicitly defined key and value types a bit more challenging.

There are a few options that might help you to deal with such a case:

* Use QuerySqlField annotation. This will prevent checking the field non-UpperCase each time because of an alias for the column is created each time the `CREATE TABLE` command being executed.
* Keeping in mind that column names converted each time to the upper case by default, you have to be sure that DDL fields and cache type fields are always match the letters case.

In the example below you can see the usage of quotes for the `affKey` CamelCase field in the `CREATE TABLE` command with matching of the same field in the `PersonKey` cache key type.

```sql
CREATE TABLE IF NOT EXISTS Person (
  id INT,
  "affKey" INT,
  val VARCHAR,
  PRIMARY KEY (id, "affKey")
) WITH "template=partitioned,backups=1,affinity_key=affKey,CACHE_NAME=Person,KEY_TYPE=PersonKey,VALUE_TYPE=PersonValue";
```

```java
class PersonKey {
    private int id;

    /*
     * This is a camel case field 'affKey' must match the DDL table schema, so you must be sure:
     * - Using the quoted "affKey" field name in the DDL table definition;
     * - Convert the 'affKey' field to the upper case 'AFFKEY' to match the DDL table definition;
     */
    @AffinityKeyMapped
    private int affKey;

    public PersonKey(int id, int affKey) {
        this.id = id;
        this.affKey = affKey;
    }
}
```

Note that some integrations with the Apache Ignite like the Spring Data `CrudRepository` doesn’t support the quoted fields to access the data.

### ALTER TABLE

Modify the structure of an existing table.

```
ALTER TABLE [IF EXISTS] tableName {alter_specification}

alter_specification:
    ADD [COLUMN] {[IF NOT EXISTS] tableColumn | (tableColumn [,...])}
  | DROP [COLUMN] {[IF EXISTS] columnName | (columnName [,...])}
  | {LOGGING | NOLOGGING}

tableColumn := columnName columnType
```

Parameters:

* `tableName` - the name of the table.
* `tableColumn` - the name and type of the column to be added to the table.
* `columnName` - the name of the column to be added or removed.
* `IF EXISTS` - if applied to TABLE, do not throw an error if a table with the specified table name does not exist. If applied to COLUMN, do not throw an error if a column with the specified name does not exist.
* `IF NOT EXISTS` - do not throw an error if a column with the same name already exists.
* `LOGGING` - enable write-ahead logging for the table. Write-ahead logging in enabled by default. The command is relevant only if Ignite persistence is used.
* `NOLOGGING` - disable write-ahead logging for the table. The command is relevant only if Ignite persistence is used.

`ALTER TABLE ADD` adds a new column or several columns to a previously created table. Once a column is added, it can be accessed using DML commands and indexed with the CREATE INDEX statement.

`ALTER TABLE DROP` removes an existing column or multiple columns from a table. Once a column is removed, it cannot be accessed within queries. Consider the following notes and limitations:

* The command does not remove actual data from the cluster which means that if the column 'name' is dropped, the value of the 'name' is still stored in the cluster. This limitation is to be addressed in the next releases.
* If the column was indexed, the index has to be dropped manually using the 'DROP INDEX' command.
* It is not possible to remove a column that is a primary key or a part of such a key.
* It is not possible to remove a column if it represents the whole value stored in the cluster. The limitation is relevant for primitive values. Ignite stores data in the form of key-value pairs and all the new columns will belong to the value. It’s not possible to change a set of columns of the key (`PRIMARY KEY`).

Both DDL and DML commands targeting the same table are blocked for a short time until `ALTER TABLE` is in progress.

Schema changes applied by this command are persisted on disk if Ignite persistence is enabled. Thus, the changes can survive full cluster restarts.

Examples:

Add a column to the table:

```
ALTER TABLE Person ADD COLUMN city varchar;
```

Add a new column to the table only if a column with the same name does not exist:

```
ALTER TABLE City ADD COLUMN IF NOT EXISTS population int;
```

Add a column​ only if the table exists:

```
ALTER TABLE IF EXISTS Missing ADD number long;
```

Add several columns to the table at once:

```
ALTER TABLE Region ADD COLUMN (code varchar, gdp double);
```

Drop a column from the table:

```
ALTER TABLE Person DROP COLUMN city;
```

Drop a column from the table only if a column with the same name does exist:

```
ALTER TABLE Person DROP COLUMN IF EXISTS population;
```

Drop a column only if the table exists:

```
ALTER TABLE IF EXISTS Person DROP COLUMN number;
```

Drop several columns from the table at once:

```
ALTER TABLE Person DROP COLUMN (code, gdp);
```

Disable write-ahead logging:

```
ALTER TABLE Person NOLOGGING
```

### DROP TABLE

The `DROP TABLE` command drops an existing table. The underlying cache with all the data in it is destroyed, too.

```
DROP TABLE [IF EXISTS] tableName
```

Parameters:

* `tableName` - the name of the table.
* `IF NOT EXISTS` - do not throw an error if a table with the same name does not exist.

Both DDL and DML commands targeting the same table are blocked while the `DROP TABLE` is in progress. Once the table is dropped, all pending commands will fail with appropriate errors.

Schema changes applied by this command are persisted on disk if Ignite persistence is enabled. Thus, the changes can survive full cluster restarts.

Examples:

Drop Person table if the one exists:

```
DROP TABLE IF EXISTS "Person";
```

### CREATE INDEX

Create an index on the specified table.

```
CREATE [SPATIAL] INDEX [[IF NOT EXISTS] indexName] ON tableName
    (columnName [ASC|DESC] [,...]) [(index_option [...])]

index_option := {INLINE_SIZE size | PARALLEL parallelism_level}
```

Parameters:

* `indexName` - the name of the index to be created.
* `ASC` - specifies ascending sort order (default).
* `DESC` - specifies descending sort order.
* `SPATIAL` - create the spatial index. Presently, only geometry types are supported.
* `IF NOT EXISTS` - do not throw an error if an index with the same name already exists. The database checks indexes' names only, and does not consider columns types or count.
* `index_option` - additional options for index creation:
  * `INLINE_SIZE` - specifies index inline size in bytes. Depending on the size, Ignite will place the whole indexed value or a part of it directly into index pages, thus omitting extra calls to data pages and increasing queries' performance. Index inlining is enabled by default and the size is pre-calculated automatically based on the table structure. To disable inlining, set the size to 0 (not recommended).
  * `PARALLEL` - specifies the number of threads to be used in parallel for index creation. The greater number is set, the faster the index is created and built. If the value exceeds the number of CPUs, then it will be decreased to the number of cores. If the parameter is not specified, then the number of threads is calculated as 25% of the CPU cores available.

`CREATE INDEX` creates a new index on the specified table. Regular indexes are stored in the internal B+tree data structures. The B+tree gets distributed across the cluster along with the actual data. A cluster node stores a part of the index for the data it owns.

If `CREATE INDEX` is executed in runtime on live data then the database will iterate over the specified columns synchronously indexing them. The rest of the DDL commands targeting the same table are blocked until CREATE INDEX is in progress. DML command execution is not affected and can be performed in parallel.

Schema changes applied by this command are persisted on disk if Ignite persistence is enabled. Thus, the changes can survive full cluster restarts.

#### Indexes Tradeoffs

There are multiple things you should consider when choosing indexes for your application.

* Indexes are not free. They consume memory, and each index needs to be updated separately, thus the performance of write operations might drop if too many indexes are created. On top of that, if a lot of indexes are defined, the optimizer might make more mistakes by choosing the wrong index while building the execution plan.
* Indexes are just sorted data structures (B+tree). If you define an index for the fields (a,b,c) then the records will be sorted first by a, then by b and only then by c.
* Indexes on single fields are no better than group indexes on multiple fields starting with the same field (index on (a) is no better than (a,b,c)). Thus it is preferable to use group indexes.
* When `INLINE_SIZE` option is specified, indexes holds a prefix of field data in the B+tree pages. This improves search performance by doing less row data retrievals, however substantially increases size of the tree (with a moderate increase in tree height) and reduces data insertion and removal performance due to excessive page splits and merges. It’s a good idea to consider page size when choosing inlining size for the tree: each B-tree entry requires `16 + inline-size` bytes in the page (plus header and extra links for the page).

Examples:

Create a regular index:

```
CREATE INDEX title_idx ON books (title);
```

Create a descending index only if it does not exist:

```
CREATE INDEX IF NOT EXISTS name_idx ON persons (firstName DESC);
```

Create a composite index:

```
CREATE INDEX city_idx ON sales (country, city);
```

Create an index specifying data inline size:

```
CREATE INDEX fast_city_idx ON sales (country, city) INLINE_SIZE 60;
```

Create a geospatial​ index:

```
CREATE SPATIAL INDEX idx_person_address ON Person (address);
```

### DROP INDEX

`DROP INDEX` deletes an existing index.

```
DROP INDEX [IF EXISTS] indexName
```

Parameters:

* `indexName` - the name of the index to drop.
* `IF EXISTS` - do not throw an error if an index with the specified name does not exist. The database checks indexes' names only not considering column types or count.

DDL commands targeting the same table are blocked until `DROP INDEX` is in progress. DML command execution is not affected and can be performed in parallel.

Schema changes applied by this command are persisted on disk if [Ignite persistence](https://ignite.apache.org/docs/2.15.0/persistence/native-persistence) is enabled. Thus, the changes can survive full cluster restarts.

#### Examples <a href="#examples" id="examples"></a>

Drop an index:

```
DROP INDEX idx_person_name;
```

### CREATE USER

The command creates a user with a given name and password.

A new user can only be created using a superuser account when authentication for thin clients is enabled. Ignite creates the superuser account under the name `ignite` and password `ignite` on the first cluster start-up. Presently, you can’t rename the superuser account nor grant its privileges to any other account.

```
CREATE USER userName WITH PASSWORD 'password';
```

Parameters:

* `userName` - new user’s name. The name cannot be longer than 60 bytes in UTF8 encoding.
* `password` - new user’s password. An empty password is not allowed.

To create a _case-sensitive_ username, use the quotation (") SQL identifier.

Examples:

Create a new user using test as a name and password:

```
CREATE USER test WITH PASSWORD 'test';
```

Create a case-sensitive username:

```
CREATE USER "TeSt" WITH PASSWORD 'test'
```

### ALTER USER

The command changes an existing user’s password. The password can be updated by the superuser (`ignite`, see CREATE USER for more details) or by the user themselves.

```
ALTER USER userName WITH PASSWORD 'newPassword';
```

Parameters:

* `userName` - existing user’s name.
* `newPassword` - the new password to set for the user’s account.

Examples:

Updating user’s password:

```
ALTER USER test WITH PASSWORD 'test123';
```

### DROP USER

The command removes an existing user.

The user can be removed only by the superuser (`ignite`, see CREATE USER for more details).

```
DROP USER userName;
```

Parameters:

* `userName` - a name of the user to remove.

Examples:

```
DROP USER test;
```

### ANALYZE

The ANALYZE command collects statistics.

```
ANALYZE 'schemaName'.'tableName'(column1, column2);
```

Parameters:

* `schemaName` - a name of the schema to collect statistics for.
* `tableName` - a name of the table to collect statistics for.
* `(column1, column2)` - names of the columns to collect statistics for.

ANALYZESCHEMA.TABLE(COLUMN,),WITH'PARAM=VALUE,';

When the ANALYZE command is used with `with` parameters statement, specified parameters are applied for every target. For example:

```
ANALYZE public.statistics_test, statistics_test2, statistics_test3(col3) WITH 'MAX_CHANGED_PARTITION_ROWS_PERCENT=15,NULLS=0'
```

Possible parameters:

* MAX\_CHANGED\_PARTITION\_ROWS\_PERCENT - Maximum percentage of outdated rows in the table (the default value is 15%). See the [SQL Statistics](https://ignite.apache.org/docs/2.15.0/SQL/sql-statistics#statistics-obsolescence) page for more details.
* NULLS - Number of null values in column.
* TOTAL - Total number of column values.
* SIZE - Average size of column values (in bytes).
* DISTINCT - Number of distinct non-null values in column.

### REFRESH STATISTICS

The command refreshes [statistics](https://ignite.apache.org/docs/2.15.0/SQL/sql-statistics).

```
REFRESH STATISTICS 'schemaName'.'tableName'(column1, column2);
```

Parameters:

* `schemaName` - a name of the schema to refresh statistics for.
* `tableName` - a name of the table to refresh statistics for.
* `(column1, column2)` - names of the columns to refresh statistics for.

REFRESHSCHEMA.TABLE(COLUMN,),

Example:

```
REFRESH STATISTICS PRODUCTS, SALE(productId, discount)
```

### DROP STATISTICS

The command drops statistics.

```
DROP STATISTICS 'schemaName'.'tableName'(column1, column2);
```

Parameters:

* `schemaName` - a name of the schema to drop statistics for.
* `tableName` - a name of the table to drop statistics for.
* `(column1, column2)` - names of the columns to drop statistics for.

DROP STATISTICSSCHEMA.TABLE(COLUMN,),

Example:

```
DROP STATISTICS USERS, ORDERS(customerId, productId)
```
