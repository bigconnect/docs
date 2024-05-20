# CREATE MATERIALIZED VIEW

### Synopsis[#](https://trino.io/docs/current/sql/create-materialized-view.html#synopsis) <a href="#synopsis" id="synopsis"></a>

```
CREATE [ OR REPLACE ] MATERIALIZED VIEW
[ IF NOT EXISTS ] view_name
[ GRACE PERIOD interval ]
[ COMMENT string ]
[ WITH properties ]
AS query
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### Description[#](https://trino.io/docs/current/sql/create-materialized-view.html#description) <a href="#description" id="description"></a>

Create and validate the definition of a new materialized view `view_name` of a SELECT `query`. You need to run the REFRESH MATERIALIZED VIEW statement after the creation to populate the materialized view with data. This materialized view is a physical manifestation of the query results at time of refresh. The data is stored, and can be referenced by future queries.

Queries accessing materialized views are typically faster than retrieving data from a view created with the same query. Any computation, aggregation, and other operation to create the data is performed once during refresh of the materialized views, as compared to each time of accessing the view. Multiple reads of view data over time, or by multiple users, all trigger repeated processing. This is avoided for materialized views.

The optional `OR REPLACE` clause causes the materialized view to be replaced if it already exists rather than raising an error.

The optional `IF NOT EXISTS` clause causes the materialized view only to be created if it does not exist yet.

Note that `OR REPLACE` and `IF NOT EXISTS` are mutually exclusive clauses.

The optional `GRACE PERIOD` clause specifies how long the query materialization is used for querying:

* Within the grace period since last refresh, data retrieval is highly performant because the query materialization is used. However, the data may not be up to date with the base tables.
* After the grace period has elapsed, the data of the materialized view is computed on-the-fly using the `query`. Retrieval is therefore slower, but the data is up to date with the base tables.
* If not specified, the grace period defaults to infinity, and therefore all queries are within the grace period.
* Every REFRESH MATERIALIZED VIEW operation resets the start time for the grace period.

The optional `COMMENT` clause causes a `string` comment to be stored with the metadata about the materialized view. The comment is displayed with the SHOW CREATE MATERIALIZED VIEW statement and is available in the table `system.metadata.materialized_view_properties`.

The optional `WITH` clause is used to define properties for the materialized view creation. Separate multiple property/value pairs by commas. The connector uses the properties as input parameters for the materialized view refresh operation. The supported properties are different for each connector and detailed in the SQL support section of the specific connector’s documentation.

After successful creation, all metadata about the materialized view is available in a system table.

### Examples[#](https://trino.io/docs/current/sql/create-materialized-view.html#examples) <a href="#examples" id="examples"></a>

Create a simple materialized view `cancelled_orders` over the `orders` table that only includes cancelled orders. Note that `orderstatus` is a numeric value that is potentially meaningless to a consumer, yet the name of the view clarifies the content:

```
CREATE MATERIALIZED VIEW cancelled_orders
AS
    SELECT orderkey, totalprice
    FROM orders
    WHERE orderstatus = 3;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Create or replace a materialized view `order_totals_by_date` that summarizes `orders` across all orders from all customers:

```
CREATE OR REPLACE MATERIALIZED VIEW order_totals_by_date
AS
    SELECT orderdate, sum(totalprice) AS price
    FROM orders
    GROUP BY orderdate;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Create a materialized view for a catalog using the Iceberg connector, with a comment and partitioning on two fields in the storage:

```
CREATE MATERIALIZED VIEW orders_nation_mkgsegment
COMMENT 'Orders with nation and market segment data'
WITH ( partitioning = ARRAY['mktsegment', 'nationkey'] )
AS
    SELECT o.*, c.nationkey, c.mktsegment
    FROM orders AS o
    JOIN customer AS c
    ON o.custkey = c.custkey;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Set multiple properties:

```
WITH ( format = 'ORC', partitioning = ARRAY['_date'] )
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Show defined materialized view properties for all catalogs:

```
SELECT * FROM system.metadata.materialized_view_properties;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Show metadata about the materialized views in all catalogs:

```
SELECT * FROM system.metadata.materialized_views;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### See also# <a href="#see-also" id="see-also"></a>

* DROP MATERIALIZED VIEW
* SHOW CREATE MATERIALIZED VIEW
* REFRESH MATERIALIZED VIEW
