# PostgreSQL

To add a database connection, click on the **gear** icon in the top right, and navigate to **Admin settings** > **Databases** > **Add a database**.

Fill out the fields for that database, and click **Save changes** at the bottom.

## Settings

You can edit these settings at any time. Just remember to save your changes.

#### Display name

The display name for the database in the Answers interface.

#### Host

Your database’s IP address, or its domain name (e.g., esc.mydatabase.com).

#### Port

The database port. E.g., 5432.

#### Database name

The name of the database you’re connecting to.

#### Username

The database username for the account that you want to use to connect to your database. You can set up multiple connections to the same database using different user accounts to connect to the same database, each with different sets of privileges.

#### Password

The password for the username that you use to connect to the database.

#### Schemas

You can specify which schemas you want to sync and scan. Options are:

* All
* Only these…
* All except…

For the **Only these** and **All except** options, you can input a comma-separated list of values to tell Metabase which schemas you want to include (or exclude). For example:

```
foo,bar,baz
```

You can use the `*` wildcard to match multiple schemas.

Let’s say you have three schemas: foo, bar, and baz.

* If you have **Only these…** set, and enter the string `b*`, you’ll sync with bar and baz.
* If you have **All except…** set, and enter the string `b*`, you’ll just sync foo.

Note that only the `*` wildcard is supported; you can’t use other special characters or regexes.

#### Use a secure connection (SSL)

Metabase automatically tries to connect to databases with SSL first, then without if that doesn’t work. If it’s possible to connect to your database with an SSL connection, Metabase will make that the default setting for your database. If you prefer to connect without this layer of security, you can always change this setting later, but we highly recommend keeping SSL turned on to keep your data secure.

**SSL Mode**

PostgreSQL databases support different levels of security with their connections, with different levels of overhead.

SSL Mode options include:

* allow
* prefer
* require
* verify-ca
* verify-full

See the PostgreSQL docs for a table about the different SSL Modes, and select the option that works for you.

**SSL root certificate (PEM)**

If you set the SSL Mode to either “verify-ca” or “verify-full”, you’ll need to specify a root certificate (PEM). You have the option of using a **Local file path** or an **Uploaded file path**.

#### Use an SSH tunnel

See our guide to SSH tunneling.

#### Authenticate client certificate

Toggle on to bring up client certificate options.

**SSL Client Certificate (PEM)**

You have the option of using a **Local file path** or an **Uploaded file path**..

**SSL Client Key (PKCS-8/DER)**

Again, you have the option of using a **Local file path** or an **Uploaded file path**.

The private key must be PKCS8 and stored in DER format.

If you instead have a PEM SSL client key, you can convert that key to the PKCS-8/DER format using [openssl](https://www.openssl.org/). The command would look something like:
