# CLI Reference

The CLI is an all in one toolkit for managing your self hosted installation. These API docs detail all of the commands and flags available to you when using the CLI.

> ### 🚧Pre-requisite: CLI installed and ready to use
>
> This guide assumes that you have installed and used the CLI before, if you have not then you can follow our guide on installing CLI.

### CLI Commands

#### hosting

Initialise and manage a self hosted installation. Currently only supports docker compose setups.

> 📘You must be in the same directory that you ran --init to use the CLI commands
>
> After running `hosting --init`, you must be in the directory where it was initialized to use the other `hosting` commands.

**Flags**

| Flag       | Purpose                                                                                                                       |
| ---------- | ----------------------------------------------------------------------------------------------------------------------------- |
| `--init`   | Initialise a self hosted docker compose setup in current directory. Pulls images and everything required to start the server. |
| `--start`  | Start the services.                                                                                                           |
| `--status` | Check the current status of all the running services.                                                                         |
| `--stop`   | Stop all the currently running services                                                                                       |
| `--update` | Pull the latest image versions required by the services                                                                       |

**Commands**

initstartstatusstopupdate

```shell
hosting --init
```

#### plugins

`plugins` is the CLI command used for creating, developing and managing custom coded components and datasources. You can find tutorials on how to create components and datasources



**Flags**

| Flag      | Purpose                                                                                                                 |
| --------- | ----------------------------------------------------------------------------------------------------------------------- |
| `--init`  | Pull down the project skeleton for a datasource or component, and initialise it in the current working directory.       |
| `--build` | Build the datasource or component in the current working directory using rollup, so it can be used inside the platform. |
| `--watch` | Watch the component/datasource in the current working directory and automatically rebuild when changes are made.        |

**Commands**

initbuildwatch

```shell
# datasource
plugins --init datasource

# component
plugins --init component
```

#### backups

`backups` is a CLI command that allows you to completely backup and restore your self hosted installation.

* Save backups as `.tar.gz` files that you can store elsewhere
* Import the `.tar.gz` files to a fresh system to restore to the state of your backup.

You can find a full end to end tutorial on using the `backups` CLI command

> 📘You need to provide credentials to the backups command
>
> If you do not pass the `--env` fiag to `backups` commands, you will have to answer a set of questions in the CLI to pass the credentials required to perform import and export of backups. The required variables are:

| Title              | Description                                                                                                       |
| ------------------ | ----------------------------------------------------------------------------------------------------------------- |
| `COUCH_DB_URL`     | Full URL for your running CouchDB service. Must include credentials. `http://<username>:<password>@<couchdb-url>` |
| `MINIO_ACCESS_KEY` | MinIO/S3 compatible object store access key                                                                       |
| `MINIO_SECRET_KEY` | MinIO/S3 compatible object store secret key                                                                       |
| `MINIO_URL`        | URL on which your MinIO/S3 compatible object store is running on.                                                 |
| `MAIN_PORT`        | The port your instance is hosted on. Defaults to 10000.                                                           |

**Flags**

| Flag       | Purpose                                                                                                                                                                                                                       |
| ---------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `--export` | Export your installation, apps and configuration to a `tar.gz` file that can be imported into another separate installation.                                                                                                  |
| `--import` | Import a `.tar.gz` file into an existing installation. If you do not pass the `--env` flag, you will need to answer some questions with database and object store passwords so it can import your backup to the other server. |
| `--env`    | Pass a path to an `.env` file that can be used to inject environment variables for either the `--import` or `--export` commands.                                                                                              |

**Commands**

exportimportenv file example

```shell
backups --export --env /path/to/env/file
```

#### analytics

This command is for opting in/out of CLI analytics.

**Flags**

| Flag       | Purpose                   |
| ---------- | ------------------------- |
| `--optIn`  | Opt in to CLI analytics.  |
| `--optOut` | Opt out of CLI analytics. |

**Commands**

optInoptOut

```shell
analytics --optIn
```
