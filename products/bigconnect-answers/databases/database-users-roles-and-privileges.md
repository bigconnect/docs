# Database users, roles, and privileges

We recommend creating a `answers` database user with the following database roles:

* `analytics` for read access to any schemas or tables used for analysis.
* Optional `answers_actions` for write access to tables used for Answers actions.
* Optional `answers_model_caching` for write access to the schema used for Answers model caching.

Bundling your privileges into roles based on use cases makes it easier to manage privileges in the future (especially in multi-tenant situations). For example, you could:

* Use the same `analytics` role for other BI tools in your data stack that need read-only access to the analytics tables in your database.
* Revoke the write access for `answers_model_caching` without affecting the write access for `answers_actions`.

### Minimum database privileges

n order to view and query your tables in Answers, you’ll have to give Answers’s database user:

* `CONNECT` to your database.
* `SELECT` privileges to any schemas or tables that you want to use in Answers.

To organize these privileges (and make maintenance easier down the line):

* Create a database role called `analytics`.
* Create a database user called `answers`.
* Add `answers` to the `analytics` role.
* Add privileges to the `analytics` role.

For example, if you’re using a Postgres database, you’d log in as an admin and run the SQL statements:

```sql
-- Create a role named "analytics".
CREATE ROLE analytics WITH LOGIN;

-- Add the CONNECT privilege to the role.
GRANT CONNECT ON DATABASE "your_database" TO analytics;

-- Create a database user named "answers".
CREATE USER answers WITH PASSWORD "your_password";

-- Give the role to the answers user.
GRANT analytics TO answers;

-- Add query privileges to the role (options 1-4):

-- Option 1: Uncomment the line below to let users with the analytics role query ALL DATA (In Postgres 14 or higher. See [Predefined Roles](https://www.postgresql.org/docs/current/predefined-roles.html#PREDEFINED-ROLES)).
-- GRANT pg_read_all_data TO analytics;

-- Option 2: Uncomment the line below to let users with the analytics role query anything in the DATABASE.
-- GRANT USAGE ON DATABASE "your_schema" TO analytics;
-- GRANT SELECT ON DATABASE "your_schema"  TO analytics;

-- Option 3: Uncomment the line below to let users with the analytics role query anything in a specific SCHEMA.
-- GRANT USAGE ON SCHEMA "your_schema" TO analytics;
-- GRANT SELECT ON ALL TABLES IN SCHEMA "your_schema" TO analytics;

-- Option 4: Uncomment the line below to let users with the analytics role query anything in a specific TABLE.
-- GRANT USAGE ON SCHEMA "your_schema" TO analytics;
-- GRANT SELECT ON "your_table" IN SCHEMA "your_schema" TO analytics;

```

Depending on how you use Answers, you can also additionally grant:

* `TEMPORARY` privileges to create temp tables.
* `EXECUTE` privileges to use stored procedures or user-defined functions.

Remember that when you grant privileges to a role, all users with that role will get those privileges.

### Grant all database privileges

If you don’t want to structure your database privileges yet:

* Create a `answers` database user.
* Give `answers` all privileges to the database.

This is a good option if you’re connecting to a local database for development or testing.

### Privileges to enable actions

Actions let Answers write back to specific tables in your database.

In addition to the minimum database privileges, you’ll need to grant write access to any tables used with actions:

* Create a new role called `answers_actions`.
* Give the role `INSERT`, `UPDATE`, and `DELETE` privileges to any tables used with Answers actions.
* Give the `answers_actions` role to the `answers` user.

```sql
-- Create a role to bundle database privileges for Answers actions.
CREATE ROLE answers_actions WITH LOGIN;

-- Grant write privileges to the TABLE used with Answers actions.
GRANT INSERT, UPDATE, DELETE ON "your_table" IN SCHEMA "your_schema" TO answers_actions;

-- Grant role to the answers user.
GRANT answers_actions TO answers;
```

### Privileges to enable model caching

Model caching lets Answers save query results to a specific schema in your database. Answers’s database user will need the `CREATE` privilege to set up the dedicated schema for model caching, as well as write access (`INSERT`, `UPDATE`, `DELETE`) to that schema.

In addition to the minimum database privileges:

* Create a new role called `answers_model_caching`.
* Give the role `CREATE` access to the database.
* Give the role `INSERT`, `UPDATE`, and `DELETE` privileges to the schema used for model caching.
* Give the `answers_model_caching` role to the `answers` user.

```sql
-- Create a role to bundle database privileges for Answers model caching.
CREATE ROLE answers_model_caching WITH LOGIN;

-- If you don't want to give CREATE access to your database,
-- add the schema manually before enabling modeling caching.
GRANT CREATE ON "database" TO answers_model_caching;

-- Grant write privileges to the SCHEMA used for model caching.
GRANT USAGE ON "your_schema" TO answers_model_caching;
GRANT INSERT, UPDATE, DELETE ON "your_model's_table" IN SCHEMA "your_schema" TO answers_model_caching;

-- Grant role to the answers user.
GRANT answers_model_caching TO answers;
```

### Privileges to enable uploads

You can upload CSVs to supported databases. Answers’s database user should have write access (`INSERT`, `UPDATE`, `DELETE`) to the schema where you want to store the uploads.

You’ll first need to create a schema to store uploads (or use an existing schema) and tell Answers that you want to use that schema to store uploads.

In addition to the minimum database privileges:

* Create a new role called `answers_uploads`.
* Give the role `INSERT`, `UPDATE`, and `DELETE` privileges to the schema where you want to store uploads.
* Give the `answers_uploads` role to the `answers` user.

```sql
-- Create a role to bundle database privileges for uploads.
CREATE ROLE answers_uploads WITH LOGIN;

-- Grant write privileges to the SCHEMA used for uploads.
GRANT USAGE ON "your_schema" TO answers_uploads;
GRANT INSERT, UPDATE, DELETE ON ALL TABLES IN SCHEMA "your_schema" TO answers_uploads;

-- Grant role to the answers user.
GRANT answers_uploads TO answers;

```

### Multi-tenant permissions

If you’re setting up multi-tenant permissions for customers who need SQL access, you can create one database connection per customer. That means each customer will connect to the database using their own database user.

Let’s say you have customers named Tangerine and Lemon:

* Create new database users `answers_tangerine` and `answers_lemon`.
* Create a `customer_facing_analytics` role with the `CONNECT` privilege.
* Create roles to bundle privileges specific to each customer’s use case. For example:
  * `tangerine_queries` to bundle read privileges for people to query and create stored procedures against the Tangerine schema.
  * `lemon_queries` to bundle read privileges for people to query tables in the Lemon schema.
  * `lemon_actions` to bundle the write privileges needed to create actions on a Lemonade table in the Lemon schema.
* Add each user to their respective roles.

```sql
-- Create one database user per customer.
CREATE USER answers_tangerine WITH PASSWORD "orange";
CREATE USER answers_lemon WITH PASSWORD "yellow";

-- Create a role to bundle privileges for all customers.
CREATE ROLE customer_facing_analytics;
GRANT CONNECT ON DATABASE "citrus" TO customer_facing_analytics;
GRANT customer_facing_analytics TO answers_tangerine, answers_lemon;

-- Create a role to bundle analytics read access for customer Tangerine.
CREATE ROLE tangerine_queries;
GRANT USAGE ON SCHEMA "tangerine" TO tangerine_queries;
GRANT SELECT, EXECUTE ON ALL TABLES IN SCHEMA "tangerine" TO tangerine_queries;
GRANT tangerine_queries TO answers_tangerine;

-- Create a role to bundle analytics read access for customer Lemon.
CREATE ROLE lemon_queries;
GRANT USAGE ON SCHEMA "lemon" TO lemon_queries;
GRANT SELECT ON ALL TABLES IN SCHEMA "lemon" TO lemon_queries;
GRANT lemon_queries TO answers_lemon;

-- Create a role to bundle privileges to Answers actions for customer Lemon.
CREATE ROLE lemon_actions;
GRANT INSERT, UPDATE, DELETE ON TABLE "lemonade" IN SCHEMA "lemon" TO lemon_actions;
GRANT lemon_actions TO answers_lemon;
```

We recommend bundling privileges into roles based on use cases per customer. That way, you can reuse common privileges across customers while still being able to grant or revoke granular privileges per customer. For example:

* If customer Tangerine needs to query the Tangerine schema from another analytics tool, you can use the `tangerine_queries` role when setting up that tool.
* If customer Lemon decides that they don’t want to use Answers actions anymore (but they still want to ask questions), you can simply revoke or drop the `lemon_actions` role.
