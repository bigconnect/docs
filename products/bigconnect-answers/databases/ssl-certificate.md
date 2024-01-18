# SSL certificate

If you’d like to connect your Answers instance to a database, you can secure the connection using Secure Socket Layer (SSL) encryption with a certificate.

Why you’d want to do this:

* You’re self-hosting Answers and want to ensure the identity of a data warehouse hosted by an external provider. You can also use this method to ensure you’re using the strictest connection parameters when connecting to your application database.

## Prerequisites

A database that allows a JDBC connection, as you’ll need to use a connection string to specify the certificate you want to use.

## Step 1: Download the root certificate from your provider

If you’re running Answers via a Docker container, you should already have the certificates for AWS and Azure.

You’ll find the certificates in the `/app/certs/` directory in Answers’s Docker image:

* AWS RDS: `/app/certs/rds-combined-ca-bundle.pem`
* Azure certificate: `/app/certs/DigiCertGlobalRootG2.crt.pem`

If you need a different certificate, you can build your own Docker image. Visit your external provider’s page for your database and find a link to download the root certificate for connecting to your database.

## Step 2: Save the certificate

Save the downloaded certificate in the same directory where you keep your answers.jar file. Technically you can store the certificate wherever, but keeping it in the same directory as your answers.jar file is a best practice. You’ll specify the certificate’s path in your connection string.

## Step 3: Add your database

For example, let’s say you want to secure a connection to a PostgreSQL database. Follow the instructions in the app to add the database. For more on setting up a database connection, check out our docs for adding a database.

## Step 4: Toggle on the “Use a secure connection (SSL)” option

If your database supports a JDBC connection, Answers will provide you with a field to input additional parameters to your connection string. Answers will use parameters in the connection string to establish a secure connection.

## Step 5: Add additional connection string options

You’ll need to specify the location of the certificate on the server that’s running Answers.

For example, when connecting to a PostgreSQL database, you’ll need to add two parameters:

* `sslmode`. You can see the full list of options in [PostgreSQL’s documentation](https://jdbc.postgresql.org/documentation/ssl/#configuring-the-client). We recommend you use `verify-full`; it’s the most secure, and overhead is minimal.
* `sslrootcert`. Here you’ll specify the file path for the certificate.

You’ll add an ampersand (`&`) to separate each parameter. For example, In the **Add additional connection string options** field, you’d add something like:

```
sslmode=verify-full&sslrootcert=/path/to/certificate.pem     
```

Replace `/path/to/certifcate.pem` with the full path for the certificate you downloaded from your provider.

You can learn more about [SSL support for PostgreSQL](https://www.postgresql.org/docs/current/libpq-ssl.html).

## Securing connection to application database using environment variables

If you’re self-hosting Answers, you can secure the connection to your application database using environment variables.

The environment variable to use is `MB_DB_CONNECTION_URI`.

You’ll need to include the full connection string here, including the db host, port, db name and user info, as well as the additional connection parameters to include the certificate. For example,

```
jdbc:postgresql://db.example.com:port/mydb?user=dbuser&password=dbpassword&ssl=true&sslmode=verify-full&sslrootcert=/path/to/certificate.pem      
```

Both can be provided to support mutual authentication scenarios.

**Truststores and keystores**

With some databases, like PostgreSQL and Oracle, you can secure connections using truststores and keystores.

**Truststores**

If a truststore is provided to verify credentials, the client (your Answers) can authenticate the server (the database) and ensure its identity is what’s expected.

**Keystores**

If a keystore is used to provide credentials, then the server (the database server) can request the client (your Answers) authenticate itself using that keystore. Keystores are used less frequently, and in some cases it’s impossible to use a keystore (Amazon’s RDS forbids keystores, for example). But you may want to use a keystore if you’re hosting on prem.
