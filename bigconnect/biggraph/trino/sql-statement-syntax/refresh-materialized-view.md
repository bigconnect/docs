# REFRESH MATERIALIZED VIEW

### Synopsis[#](https://trino.io/docs/current/sql/refresh-materialized-view.html#synopsis) <a href="#synopsis" id="synopsis"></a>

```
REFRESH MATERIALIZED VIEW view_name
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### Description[#](https://trino.io/docs/current/sql/refresh-materialized-view.html#description) <a href="#description" id="description"></a>

Initially populate or refresh the data stored in the materialized view `view_name`. The materialized view must be defined with CREATE MATERIALIZED VIEW. Data is retrieved from the underlying tables accessed by the defined query.

The initial population of the materialized view is typically processing intensive since it reads the data from the source tables and performs physical write operations.

The refresh operation can be less intensive, if the underlying data has not changed and the connector has implemented a mechanism to be aware of that. The specific implementation and performance varies by connector used to create the materialized view.

### See also# <a href="#see-also" id="see-also"></a>

* CREATE MATERIALIZED VIEW
* DROP MATERIALIZED VIEW
* SHOW CREATE MATERIALIZED VIEW
