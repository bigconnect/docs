# RESET SESSION

### Synopsis[#](https://trino.io/docs/current/sql/reset-session.html#synopsis) <a href="#synopsis" id="synopsis"></a>

```
RESET SESSION name
RESET SESSION catalog.name
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### Description[#](https://trino.io/docs/current/sql/reset-session.html#description) <a href="#description" id="description"></a>

Reset a [session property](https://trino.io/docs/current/sql/set-session.html#session-properties-definition) value to the default value.

### Examples[#](https://trino.io/docs/current/sql/reset-session.html#examples) <a href="#examples" id="examples"></a>

```
RESET SESSION optimize_hash_generation;
RESET SESSION hive.optimized_reader_enabled;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### See also[#](https://trino.io/docs/current/sql/reset-session.html#see-also) <a href="#see-also" id="see-also"></a>

[SET SESSION](https://trino.io/docs/current/sql/set-session.html), [SHOW SESSION](https://trino.io/docs/current/sql/show-session.html)

\
