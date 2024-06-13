# Resource groups

Resource groups place limits on resource usage, and can enforce queueing policies on queries that run within them, or divide their resources among sub-groups. A query belongs to a single resource group, and consumes resources from that group (and its ancestors). Except for the limit on queued queries, when a resource group runs out of a resource it does not cause running queries to fail; instead new queries become queued. A resource group may have sub-groups or may accept queries, but may not do both.

The resource groups and associated selection rules are configured by a manager, which is pluggable.

You can use a file-based or a database-based resource group manager:

* Add a file `etc/resource-groups.properties`
* Set the `resource-groups.configuration-manager` property to `file` or `db`
* Add further configuration properties for the desired manager.

### File resource group manager <a href="#file-resource-group-manager" id="file-resource-group-manager"></a>

The file resource group manager reads a JSON configuration file, specified with `resource-groups.config-file`:

```
resource-groups.configuration-manager=file
resource-groups.config-file=etc/resource-groups.json
```



The path to the JSON file can be an absolute path, or a path relative to the Trino data directory. The JSON file only needs to be present on the coordinator.

### Database resource group manager <a href="#database-resource-group-manager" id="database-resource-group-manager"></a>

The database resource group manager loads the configuration from a relational database. The supported databases are MySQL, PostgreSQL, and Oracle.

```
resource-groups.configuration-manager=db
resource-groups.config-db-url=jdbc:mysql://localhost:3306/resource_groups
resource-groups.config-db-user=username
resource-groups.config-db-password=password
```



The resource group configuration must be populated through tables `resource_groups_global_properties`, `resource_groups`, and `selectors`. If any of the tables do not exist when Trino starts, they will be created automatically.

The rules in the `selectors` table are processed in descending order of the values in the `priority` field.

The `resource_groups` table also contains an `environment` field which is matched with the value contained in the `node.environment` property in Node properties. This allows the resource group configuration for different Trino clusters to be stored in the same database if required.

The configuration is reloaded from the database every second, and the changes are reflected automatically for incoming queries.

| Property name                                  | Description                                                                                                                                                                                                                                                                                                                               | Default value |
| ---------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------- |
| `resource-groups.config-db-url`                | Database URL to load configuration from.                                                                                                                                                                                                                                                                                                  | `none`        |
| `resource-groups.config-db-user`               | Database user to connect with.                                                                                                                                                                                                                                                                                                            | `none`        |
| `resource-groups.config-db-password`           | Password for database user to connect with.                                                                                                                                                                                                                                                                                               | `none`        |
| `resource-groups.max-refresh-interval`         | The maximum time period for which the cluster will continue to accept queries after refresh failures, causing configuration to become stale.                                                                                                                                                                                              | `1h`          |
| `resource-groups.refresh-interval`             | How often the cluster reloads from the database                                                                                                                                                                                                                                                                                           | `1s`          |
| `resource-groups.exact-match-selector-enabled` | Setting this flag enables usage of an additional `exact_match_source_selectors` table to configure resource group selection rules defined exact name based matches for source, environment and query type. By default, the rules are only loaded from the `selectors` table, with a regex-based filter for `source`, among other filters. | `false`       |

### Resource group properties <a href="#resource-group-properties" id="resource-group-properties"></a>

* `name` (required): name of the group. May be a template (see below).
* `maxQueued` (required): maximum number of queued queries. Once this limit is reached new queries are rejected.
* `softConcurrencyLimit` (optional): number of concurrently running queries after which new queries will only run if all peer resource groups below their soft limits are ineligible or if all eligible peers are above soft limits.
* `hardConcurrencyLimit` (required): maximum number of running queries.
* `softMemoryLimit` (required): maximum amount of distributed memory this group may use, before new queries become queued. May be specified as an absolute value (i.e. `1GB`) or as a percentage (i.e. `10%`) of the cluster’s memory.
* `softCpuLimit` (optional): maximum amount of CPU time this group may use in a period (see `cpuQuotaPeriod`), before a penalty is applied to the maximum number of running queries. `hardCpuLimit` must also be specified.
* `hardCpuLimit` (optional): maximum amount of CPU time this group may use in a period.
* `schedulingPolicy` (optional): specifies how queued queries are selected to run, and how sub-groups become eligible to start their queries. May be one of three values:
  * `fair` (default): queued queries are processed first-in-first-out, and sub-groups must take turns starting new queries, if they have any queued.
  * `weighted_fair`: sub-groups are selected based on their `schedulingWeight` and the number of queries they are already running concurrently. The expected share of running queries for a sub-group is computed based on the weights for all currently eligible sub-groups. The sub-group with the least concurrency relative to its share is selected to start the next query.
  * `weighted`: queued queries are selected stochastically in proportion to their priority, specified via the `query_priority` session property. Sub groups are selected to start new queries in proportion to their `schedulingWeight`.
  * `query_priority`: all sub-groups must also be configured with `query_priority`. Queued queries are selected strictly according to their priority.
* `schedulingWeight` (optional): weight of this sub-group used in `weighted` and the `weighted_fair` scheduling policy. Defaults to `1`. See Scheduling weight example.
* `jmxExport` (optional): If true, group statistics are exported to JMX for monitoring. Defaults to `true`.
* `subGroups` (optional): list of sub-groups.

#### Scheduling weight example <a href="#scheduling-weight-example" id="scheduling-weight-example"></a>

Schedule weighting is a method of assigning a priority to a resource. Sub-groups with a higher scheduling weight are given higher priority. For example, to ensure timely execution of scheduled pipelines queries, weight them higher than adhoc queries.

In the following example, pipeline queries are weighted with a value of `350`, which is higher than the adhoc queries that have a scheduling weight of `150`. This means that approximately 70% (350 out of 500 queries) of your queries come from the pipeline sub-group, and 30% (150 out of 500 queries) come from the adhoc sub-group in a given timeframe. Alternatively, if you set each sub-group value to `1`, the weight of the queries for the pipeline and adhoc sub-groups are split evenly and each receive 50% of the queries in a given timeframe.

```
{
  {
    "name": "pipeline",
    "schedulingWeight": 350,
  },
  {
    "name": "adhoc",
    "schedulingWeight": 150
  }
}
```



### Selector rules <a href="#selector-rules" id="selector-rules"></a>

The selector rules for pattern matching use Java’s regular expression capabilities. Java implements regular expressions through the `java.util.regex` package. For more information, see the Java documentation.

* `user` (optional): Java regex to match against user name.
* `userGroup` (optional): Java regex to match against every user group the user belongs to.
* `source` (optional): Java regex to match against source string.
* `queryType` (optional): string to match against the type of the query submitted:
  * `SELECT`: `SELECT` queries.
  * `EXPLAIN`: `EXPLAIN` queries (but not `EXPLAIN ANALYZE`).
  * `DESCRIBE`: `DESCRIBE`, `DESCRIBE INPUT`, `DESCRIBE OUTPUT`, and `SHOW` queries.
  * `INSERT`: `INSERT`, `CREATE TABLE AS`, and `REFRESH MATERIALIZED VIEW` queries.
  * `UPDATE`: `UPDATE` queries.
  * `DELETE`: `DELETE` queries.
  * `ANALYZE`: `ANALYZE` queries.
  * `DATA_DEFINITION`: Queries that alter/create/drop the metadata of schemas/tables/views, and that manage prepared statements, privileges, sessions, and transactions.
* `clientTags` (optional): list of tags. To match, every tag in this list must be in the list of client-provided tags associated with the query.
* `group` (required): the group these queries will run in.

All rules within a single selector are combined using a logical `AND`. Therefore all rules must match for a selector to be applied.

Selectors are processed sequentially and the first one that matches will be used.

### Global properties <a href="#global-properties" id="global-properties"></a>

* `cpuQuotaPeriod` (optional): the period in which cpu quotas are enforced.

### Providing selector properties <a href="#providing-selector-properties" id="providing-selector-properties"></a>

The source name can be set as follows:

* CLI: use the `--source` option.
* JDBC driver when used in client apps: add the `source` property to the connection configuration and set the value when using a Java application that uses the JDBC Driver.
* JDBC driver used with Java programs: add a property with the key `source` and the value on the `Connection` instance as shown in the example.

Client tags can be set as follows:

* CLI: use the `--client-tags` option.
* JDBC driver when used in client apps: add the `clientTags` property to the connection configuration and set the value when using a Java application that uses the JDBC Driver.
* JDBC driver used with Java programs: add a property with the key `clientTags` and the value on the `Connection` instance as shown in the example.

### Example <a href="#example" id="example"></a>

In the example configuration below, there are several resource groups, some of which are templates. Templates allow administrators to construct resource group trees dynamically. For example, in the `pipeline_${USER}` group, `${USER}` is expanded to the name of the user that submitted the query. `${SOURCE}` is also supported, which is expanded to the source that submitted the query. You may also use custom named variables in the `source` and `user` regular expressions.

There are four selectors, that define which queries run in which resource group:

* The first selector matches queries from `bob` and places them in the admin group.
* The second selector matches queries from `admin` user group and places them in the admin group.
* The third selector matches all data definition (DDL) queries from a source name that includes `pipeline` and places them in the `global.data_definition` group. This could help reduce queue times for this class of queries, since they are expected to be fast.
* The fourth selector matches queries from a source name that includes `pipeline`, and places them in a dynamically-created per-user pipeline group under the `global.pipeline` group.
* The fifth selector matches queries that come from BI tools which have a source matching the regular expression `jdbc#(?<toolname>.*)` and have client provided tags that are a superset of `hipri`. These are placed in a dynamically-created sub-group under the `global.adhoc` group. The dynamic sub-groups are created based on the values of named variables `toolname` and `user`. The values are derived from the source regular expression and the query user respectively. Consider a query with a source `jdbc#powerfulbi`, user `kayla`, and client tags `hipri` and `fast`. This query is routed to the `global.adhoc.bi-powerfulbi.kayla` resource group.
* The last selector is a catch-all, which places all queries that have not yet been matched into a per-user adhoc group.

Together, these selectors implement the following policy:

* The user `bob` and any user belonging to user group `admin` is an admin and can run up to 50 concurrent queries. Queries will be run based on user-provided priority.

For the remaining users:

* No more than 100 total queries may run concurrently.
* Up to 5 concurrent DDL queries with a source `pipeline` can run. Queries are run in FIFO order.
* Non-DDL queries will run under the `global.pipeline` group, with a total concurrency of 45, and a per-user concurrency of 5. Queries are run in FIFO order.
* For BI tools, each tool can run up to 10 concurrent queries, and each user can run up to 3. If the total demand exceeds the limit of 10, the user with the fewest running queries gets the next concurrency slot. This policy results in fairness when under contention.
* All remaining queries are placed into a per-user group under `global.adhoc.other` that behaves similarly.

#### File resource group manager <a href="#id1" id="id1"></a>

```
{
  "rootGroups": [
    {
      "name": "global",
      "softMemoryLimit": "80%",
      "hardConcurrencyLimit": 100,
      "maxQueued": 1000,
      "schedulingPolicy": "weighted",
      "jmxExport": true,
      "subGroups": [
        {
          "name": "data_definition",
          "softMemoryLimit": "10%",
          "hardConcurrencyLimit": 5,
          "maxQueued": 100,
          "schedulingWeight": 1
        },
        {
          "name": "adhoc",
          "softMemoryLimit": "10%",
          "hardConcurrencyLimit": 50,
          "maxQueued": 1,
          "schedulingWeight": 10,
          "subGroups": [
            {
              "name": "other",
              "softMemoryLimit": "10%",
              "hardConcurrencyLimit": 2,
              "maxQueued": 1,
              "schedulingWeight": 10,
              "schedulingPolicy": "weighted_fair",
              "subGroups": [
                {
                  "name": "${USER}",
                  "softMemoryLimit": "10%",
                  "hardConcurrencyLimit": 1,
                  "maxQueued": 100
                }
              ]
            },
            {
              "name": "bi-${toolname}",
              "softMemoryLimit": "10%",
              "hardConcurrencyLimit": 10,
              "maxQueued": 100,
              "schedulingWeight": 10,
              "schedulingPolicy": "weighted_fair",
              "subGroups": [
                {
                  "name": "${USER}",
                  "softMemoryLimit": "10%",
                  "hardConcurrencyLimit": 3,
                  "maxQueued": 10
                }
              ]
            }
          ]
        },
        {
          "name": "pipeline",
          "softMemoryLimit": "80%",
          "hardConcurrencyLimit": 45,
          "maxQueued": 100,
          "schedulingWeight": 1,
          "jmxExport": true,
          "subGroups": [
            {
              "name": "pipeline_${USER}",
              "softMemoryLimit": "50%",
              "hardConcurrencyLimit": 5,
              "maxQueued": 100
            }
          ]
        }
      ]
    },
    {
      "name": "admin",
      "softMemoryLimit": "100%",
      "hardConcurrencyLimit": 50,
      "maxQueued": 100,
      "schedulingPolicy": "query_priority",
      "jmxExport": true
    }
  ],
  "selectors": [
    {
      "user": "bob",
      "group": "admin"
    },
    {
      "userGroup": "admin",
      "group": "admin"
    },
    {
      "source": ".*pipeline.*",
      "queryType": "DATA_DEFINITION",
      "group": "global.data_definition"
    },
    {
      "source": ".*pipeline.*",
      "group": "global.pipeline.pipeline_${USER}"
    },
    {
      "source": "jdbc#(?<toolname>.*)",
      "clientTags": ["hipri"],
      "group": "global.adhoc.bi-${toolname}.${USER}"
    },
    {
      "group": "global.adhoc.other.${USER}"
    }
  ],
  "cpuQuotaPeriod": "1h"
}
```

#### Database resource group manager <a href="#id2" id="id2"></a>

This example is for a MySQL database.

```
-- global properties
INSERT INTO resource_groups_global_properties (name, value) VALUES ('cpu_quota_period', '1h');

-- Every row in resource_groups table indicates a resource group.
-- The enviroment name is 'test_environment', make sure it matches `node.environment` in your cluster.
-- The parent-child relationship is indicated by the ID in 'parent' column.

-- create a root group 'global' with NULL parent
INSERT INTO resource_groups (name, soft_memory_limit, hard_concurrency_limit, max_queued, scheduling_policy, jmx_export, environment) VALUES ('global', '80%', 100, 1000, 'weighted', true, 'test_environment');

-- get ID of 'global' group
SELECT resource_group_id FROM resource_groups WHERE name = 'global';  -- 1
-- create two new groups with 'global' as parent
INSERT INTO resource_groups (name, soft_memory_limit, hard_concurrency_limit, max_queued, scheduling_weight, environment, parent) VALUES ('data_definition', '10%', 5, 100, 1, 'test_environment', 1);
INSERT INTO resource_groups (name, soft_memory_limit, hard_concurrency_limit, max_queued, scheduling_weight, environment, parent) VALUES ('adhoc', '10%', 50, 1, 10, 'test_environment', 1);

-- get ID of 'adhoc' group
SELECT resource_group_id FROM resource_groups WHERE name = 'adhoc';   -- 3
-- create 'other' group with 'adhoc' as parent
INSERT INTO resource_groups (name, soft_memory_limit, hard_concurrency_limit, max_queued, scheduling_weight, scheduling_policy, environment, parent) VALUES ('other', '10%', 2, 1, 10, 'weighted_fair', 'test_environment', 3);

-- get ID of 'other' group
SELECT resource_group_id FROM resource_groups WHERE name = 'other';  -- 4
-- create '${USER}' group with 'other' as parent.
INSERT INTO resource_groups (name, soft_memory_limit, hard_concurrency_limit, max_queued, environment, parent) VALUES ('${USER}', '10%', 1, 100, 'test_environment', 4);

-- create 'bi-${toolname}' group with 'adhoc' as parent
INSERT INTO resource_groups (name, soft_memory_limit, hard_concurrency_limit, max_queued, scheduling_weight, scheduling_policy, environment, parent) VALUES ('bi-${toolname}', '10%', 10, 100, 10, 'weighted_fair', 'test_environment', 3);

-- get ID of 'bi-${toolname}' group
SELECT resource_group_id FROM resource_groups WHERE name = 'bi-${toolname}';  -- 6
-- create '${USER}' group with 'bi-${toolname}' as parent. This indicates
-- nested group 'global.adhoc.bi-${toolname}.${USER}', and will have a
-- different ID than 'global.adhoc.other.${USER}' created above.
INSERT INTO resource_groups (name, soft_memory_limit, hard_concurrency_limit, max_queued,  environment, parent) VALUES ('${USER}', '10%', 3, 10, 'test_environment', 6);

-- create 'pipeline' group with 'global' as parent
INSERT INTO resource_groups (name, soft_memory_limit, hard_concurrency_limit, max_queued, scheduling_weight, jmx_export, environment, parent) VALUES ('pipeline', '80%', 45, 100, 1, true, 'test_environment', 1);

-- get ID of 'pipeline' group
SELECT resource_group_id FROM resource_groups WHERE name = 'pipeline'; -- 8
-- create 'pipeline_${USER}' group with 'pipeline' as parent
INSERT INTO resource_groups (name, soft_memory_limit, hard_concurrency_limit, max_queued,  environment, parent) VALUES ('pipeline_${USER}', '50%', 5, 100, 'test_environment', 8);

-- create a root group 'admin' with NULL parent
INSERT INTO resource_groups (name, soft_memory_limit, hard_concurrency_limit, max_queued, scheduling_policy, environment, jmx_export) VALUES ('admin', '100%', 50, 100, 'query_priority', 'test_environment', true);


-- Selectors

-- use ID of 'admin' resource group for selector
INSERT INTO selectors (resource_group_id, user_regex, priority) VALUES ((SELECT resource_group_id FROM resource_groups WHERE name = 'admin'), 'bob', 6);

-- use ID of 'admin' resource group for selector
INSERT INTO selectors (resource_group_id, user_group_regex, priority) VALUES ((SELECT resource_group_id FROM resource_groups WHERE name = 'admin'), 'admin', 5);

-- use ID of 'global.data_definition' resource group for selector
INSERT INTO selectors (resource_group_id, source_regex, query_type, priority) VALUES ((SELECT resource_group_id FROM resource_groups WHERE name = 'data_definition'), '.*pipeline.*', 'DATA_DEFINITION', 4);

-- use ID of 'global.pipeline.pipeline_${USER}' resource group for selector
INSERT INTO selectors (resource_group_id, source_regex, priority) VALUES ((SELECT resource_group_id FROM resource_groups WHERE name = 'pipeline_${USER}'), '.*pipeline.*', 3);

-- get ID of 'global.adhoc.bi-${toolname}.${USER}' resource group by disambiguating group name using parent ID
SELECT A.resource_group_id self_id, B.resource_group_id parent_id, concat(B.name, '.', A.name) name_with_parent
FROM resource_groups A JOIN resource_groups B ON A.parent = B.resource_group_id
WHERE A.name = '${USER}' AND B.name = 'bi-${toolname}';
--  7 |         6 | bi-${toolname}.${USER}
INSERT INTO selectors (resource_group_id, source_regex, client_tags, priority) VALUES (7, 'jdbc#(?<toolname>.*)', '["hipri"]', 2);

-- get ID of 'global.adhoc.other.${USER}' resource group for by disambiguating group name using parent ID
SELECT A.resource_group_id self_id, B.resource_group_id parent_id, concat(B.name, '.', A.name) name_with_parent
FROM resource_groups A JOIN resource_groups B ON A.parent = B.resource_group_id
WHERE A.name = '${USER}' AND B.name = 'other';
-- |       5 |         4 | other.${USER}    |
INSERT INTO selectors (resource_group_id, priority) VALUES (5, 1);
```
