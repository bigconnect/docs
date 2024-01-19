# Vertica

## Downloading the Vertica JDBC Driver JAR

You can download the JDBC driver from [Vertica’s JDBC driver downloads page](https://my.vertica.com/download/vertica/client-drivers/). Head to this page, log in to your account, accept the license agreement, and download `vertica-jdbc-8.0.0-0.jar` (for Vertica DB version 8.0) or whatever driver version most closely matches the version of Vertica you’re running.

It’s very important to make sure you use the correct version of the JDBC driver; version 8.0 of the driver won’t work with Vertica version 7.2; version 7.2 of the driver won’t work with Vertica version 7.1, and so forth. If in doubt, consult Vertica’s documentation to find the correct version of the JDBC driver for your version of Vertica.

## Adding the Vertica JDBC Driver JAR to the Asnwers Plugins Directory

Answers will automatically make the Vertica driver available if it finds the Vertica JDBC driver JAR in the Answers plugins directory when it starts up. All you need to do is create the directory, move the JAR you just downloaded into it, and restart Answers.

By default, the plugins directory is called `plugins`, and lives in the same directory as the Answers JAR.

For example, if you’re running Answers from a directory called `/app/`, you should move the Vertica JDBC driver JAR to `/app/plugins/`:

```
# example directory structure for running Answers with Vertica support
/app/answers.jar
/app/plugins/vertica-jdbc-8.0.0-0.jar
```

