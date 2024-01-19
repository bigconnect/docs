# CLI

The Trino CLI provides a terminal-based, interactive shell for running queries. The CLI is a self-executing JAR file, which means it acts like a normal UNIX executable

### Requirements <a id="requirements"></a>

The CLI requires a Java virtual machine available on the path. It can be used with Java version 8 and higher.

### Installation

Download [trino-cli-355-executable.jar](https://repo1.maven.org/maven2/io/trino/trino-cli/355/trino-cli-355-executable.jar),  rename it to `trino` make it executable with `chmod +x`, then run it:

```text
./trino --server https://dsql-<TENANT_ID>.cloud.bigconnect.io --user <TENANT_ID> --catalog bigconnect --schema public --password
```

where &lt;TENANT\_ID&gt; is your cloud tenant id that can be found on the summary page of your Project in the [Cloud console](https://console.cloud.bigconnect.io).

Run the CLI with the `--help` option to see the available options.

### Authentication

You can override your username with the `--user` option. It defaults to your operating system username. If your Trino server requires password authentication, use the `--password` option to have the CLI prompt for a password. You can set the `PRESTO_PASSWORD` environment variable with the password value to avoid the prompt.

### Pagination <a id="pagination"></a>

By default, the results of queries are paginated using the `less` program which is configured with a carefully selected set of options. This behavior can be overridden by setting the environment variable `PRESTO_PAGER` to the name of a different program such as `more` or [pspg](https://github.com/okbob/pspg), or it can be set to an empty value to completely disable pagination.

