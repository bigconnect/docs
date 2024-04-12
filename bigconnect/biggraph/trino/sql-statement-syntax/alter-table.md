# ALTER TABLE

### Synopsis[#](https://trino.io/docs/current/sql/alter-table.html#synopsis) <a href="#synopsis" id="synopsis"></a>

```
ALTER TABLE [ IF EXISTS ] name RENAME TO new_name
ALTER TABLE [ IF EXISTS ] name ADD COLUMN [ IF NOT EXISTS ] column_name data_type
  [ NOT NULL ] [ COMMENT comment ]
  [ WITH ( property_name = expression [, ...] ) ]
ALTER TABLE [ IF EXISTS ] name DROP COLUMN [ IF EXISTS ] column_name
ALTER TABLE [ IF EXISTS ] name RENAME COLUMN [ IF EXISTS ] old_name TO new_name
ALTER TABLE [ IF EXISTS ] name ALTER COLUMN column_name SET DATA TYPE new_type
ALTER TABLE [ IF EXISTS ] name ALTER COLUMN column_name DROP NOT NULL
ALTER TABLE name SET AUTHORIZATION ( user | USER user | ROLE role )
ALTER TABLE name SET PROPERTIES property_name = expression [, ...]
ALTER TABLE name EXECUTE command [ ( parameter => expression [, ... ] ) ]
    [ WHERE expression ]
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### Description[#](https://trino.io/docs/current/sql/alter-table.html#description) <a href="#description" id="description"></a>

Change the definition of an existing table.

The optional `IF EXISTS` (when used before the table name) clause causes the error to be suppressed if the table does not exists.

The optional `IF EXISTS` (when used before the column name) clause causes the error to be suppressed if the column does not exists.

The optional `IF NOT EXISTS` clause causes the error to be suppressed if the column already exists.

#### SET PROPERTIES[#](https://trino.io/docs/current/sql/alter-table.html#set-properties) <a href="#set-properties" id="set-properties"></a>

The `ALTER TABLE SET PROPERTIES` statement followed by some number of `property_name` and `expression` pairs applies the specified properties and values to a table. Ommitting an already-set property from this statement leaves that property unchanged in the table.

A property in a `SET PROPERTIES` statement can be set to `DEFAULT`, which reverts its value back to the default in that table.

Support for `ALTER TABLE SET PROPERTIES` varies between connectors, as not all connectors support modifying table properties.

#### EXECUTE[#](https://trino.io/docs/current/sql/alter-table.html#execute) <a href="#execute" id="execute"></a>

The `ALTER TABLE EXECUTE` statement followed by a `command` and `parameters` modifies the table according to the specified command and parameters. `ALTER TABLE EXECUTE` supports different commands on a per-connector basis.

You can use the `=>` operator for passing named parameter values. The left side is the name of the parameter, the right side is the value being passed.

Executable commands are contributed by connectors, such as the `optimize` command provided by the [Hive](https://trino.io/docs/current/connector/hive.html#hive-alter-table-execute), [Delta Lake](https://trino.io/docs/current/connector/delta-lake.html#delta-lake-alter-table-execute), and [Iceberg](https://trino.io/docs/current/connector/iceberg.html#iceberg-alter-table-execute) connectors. For example, a user observing many small files in the storage of a table called `test_table` in the `test` schema of the `example` catalog, can use the `optimize` command to merge all files below the `file_size_threshold` value. The result is fewer, but larger files, which typically results in higher query performance on the data in the files:

```
ALTER TABLE example.test.test_table EXECUTE optimize(file_size_threshold => '16MB')
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### Examples[#](https://trino.io/docs/current/sql/alter-table.html#examples) <a href="#examples" id="examples"></a>

Rename table `users` to `people`:

```
ALTER TABLE users RENAME TO people;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Rename table `users` to `people` if table `users` exists:

```
ALTER TABLE IF EXISTS users RENAME TO people;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Add column `zip` to the `users` table:

```
ALTER TABLE users ADD COLUMN zip varchar;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Add column `zip` to the `users` table if table `users` exists and column `zip` not already exists:

```
ALTER TABLE IF EXISTS users ADD COLUMN IF NOT EXISTS zip varchar;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Drop column `zip` from the `users` table:

```
ALTER TABLE users DROP COLUMN zip;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Drop column `zip` from the `users` table if table `users` and column `zip` exists:

```
ALTER TABLE IF EXISTS users DROP COLUMN IF EXISTS zip;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Rename column `id` to `user_id` in the `users` table:

```
ALTER TABLE users RENAME COLUMN id TO user_id;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Rename column `id` to `user_id` in the `users` table if table `users` and column `id` exists:

```
ALTER TABLE IF EXISTS users RENAME column IF EXISTS id to user_id;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Change type of column `id` to `bigint` in the `users` table:

```
ALTER TABLE users ALTER COLUMN id SET DATA TYPE bigint;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Drop a not null constraint on `id` column in the `users` table:

```
ALTER TABLE users ALTER COLUMN id DROP NOT NULL;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Change owner of table `people` to user `alice`:

```
ALTER TABLE people SET AUTHORIZATION alice
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Allow everyone with role public to drop and alter table `people`:

```
ALTER TABLE people SET AUTHORIZATION ROLE PUBLIC
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Set table properties (`x = y`) in table `people`:

```
ALTER TABLE people SET PROPERTIES x = 'y';
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Set multiple table properties (`foo = 123` and `foo bar = 456`) in table `people`:

```
ALTER TABLE people SET PROPERTIES foo = 123, "foo bar" = 456;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Set table property `x` to its default value in table\`\`people\`\`:

```
ALTER TABLE people SET PROPERTIES x = DEFAULT;
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### See also[#](https://trino.io/docs/current/sql/alter-table.html#see-also) <a href="#see-also" id="see-also"></a>

[CREATE TABLE](https://trino.io/docs/current/sql/create-table.html)
