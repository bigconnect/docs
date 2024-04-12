# SET TIME ZONE

### Synopsis[#](https://trino.io/docs/current/sql/set-time-zone.html#synopsis) <a href="#synopsis" id="synopsis"></a>

```
SET TIME ZONE LOCAL
SET TIME ZONE expression
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### Description[#](https://trino.io/docs/current/sql/set-time-zone.html#description) <a href="#description" id="description"></a>

Sets the default time zone for the current session.

If the `LOCAL` option is specified, the time zone for the current session is set to the initial time zone of the session.

If the `expression` option is specified:

* if the type of the `expression` is a string, the time zone for the current session is set to the corresponding region-based time zone ID or the corresponding zone offset.
* if the type of the `expression` is an interval, the time zone for the current session is set to the corresponding zone offset relative to UTC. It must be in the range of \[-14,14] hours.

### Examples[#](https://trino.io/docs/current/sql/set-time-zone.html#examples) <a href="#examples" id="examples"></a>

Use the default time zone for the current session:

```
SET TIME ZONE LOCAL;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Use a zone offset for specifying the time zone:

```
SET TIME ZONE '-08:00';
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Use an interval literal for specifying the time zone:

```
SET TIME ZONE INTERVAL '10' HOUR;
SET TIME ZONE INTERVAL -'08:00' HOUR TO MINUTE;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Use a region-based time zone identifier for specifying the time zone:

```
SET TIME ZONE 'America/Los_Angeles';
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

The time zone identifier to be used can be passed as the output of a function call:

```
SET TIME ZONE concat_ws('/', 'America', 'Los_Angeles');
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### Limitations[#](https://trino.io/docs/current/sql/set-time-zone.html#limitations) <a href="#limitations" id="limitations"></a>

Setting the default time zone for the session has no effect if the `sql.forced-session-time-zone` configuration property is already set.

### See also[#](https://trino.io/docs/current/sql/set-time-zone.html#see-also) <a href="#see-also" id="see-also"></a>

* [`current_timezone()`](https://trino.io/docs/current/functions/datetime.html#current\_timezone)

\
