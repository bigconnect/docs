# CLI Install

The CLI tool can be used to create a new installation, to manage an existing installation, make backups or update a installation to a new version. The video below summarizes the tool with further instructions below.

### Install CLI

#### Prerequisites

You should ensure you have Node and Docker installed on your system. An example is shown below for Linux (Debian/Ubuntu). On MacOSX consider using [brew](https://changelog.com/posts/install-node-js-with-homebrew-on-os-x) and on Windows consider using [chocolatey](https://vocon-it.com/2019/11/19/install-npm-using-chocolatey-on-windows/).

```
# These commands can be used to install the CLI on a debian/ubuntu distribution
sudo apt install docker.io
curl -sL https://deb.nodesource.com/setup_18.x | sudo bash -
npm install -g /cli
```

#### Install CLI with NPM

If you already have NodeJS version 20+ on your system you can install the CLI tool with NPM as shown below. A `yarn` alternative is shown for those who prefer to use `yarn`:

npmyarn

```Text
npm install -g @cli
```

Install using the CLI

Use the following command to generate a `.env` file containing the secrets to be used when you start your installation:

```
hosting --init
```

The commands below can be viewed in this recipe if desired:

🤖CLI setup \[NPM]Open Recipe

You can view, copy or backup your `.env` file at this point:

<figure><img src="https://files.readme.io/74b2b8f-image.png" alt=""><figcaption></figcaption></figure>

You can now start the services with the following command:

```
hosting --start
```

After a little time you will be able to view the running docker containers with:

```
docker ps
docker-compose ps
```

And you should be able to access it over HTTP on the port number you chose during initialisation e.g. [http://127.0.0.1:10000](http://127.0.0.1:10000/)

#### Update Version using the CLI

To update the services with the latest docker images, use the following command

```
hosting --update
```

#### Backing Up Data using the CLI

This command can be run from within the same directory as your `.env` file:

```
backups --export --env .env
```
