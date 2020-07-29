# CLI

The DataCharm CLI provides a terminal-based, interactive shell for running queries. The CLI is a self-executing JAR file, which means it acts like a normal UNIX executable

### Requirements <a id="requirements"></a>

The CLI requires a Java virtual machine available on the path. It can be used with Java version 8 and higher.

### Installation

Download [datacharm-cli-340.jar](http://bits.bigconnect.io/cloud/datacharm-cli-340.jar),  make it executable with `chmod +x`, then run it:

```text
./datacharm-cli-340.jar --server <datacharm_url> --catalog bigconnect --schema public
```

Run the CLI with the `--help` option to see the available options.

### Authentication

You can override your username with the `--user` option. It defaults to your operating system username. If your Presto server requires password authentication, use the `--password` option to have the CLI prompt for a password. You can set the `PRESTO_PASSWORD` environment variable with the password value to avoid the prompt.

### Pagination <a id="pagination"></a>

By default, the results of queries are paginated using the `less` program which is configured with a carefully selected set of options. This behavior can be overridden by setting the environment variable `PRESTO_PAGER` to the name of a different program such as `more` or [pspg](https://github.com/okbob/pspg), or it can be set to an empty value to completely disable pagination.

