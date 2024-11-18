# Docker compose

The first step to getting your platform up and running with [Docker](https://www.docker.com/) is to make sure that you have the following installed:

1. [Docker Engine](https://docs.docker.com/engine/install/) - for Windows and OSX this means installing the Desktop client, for Linux follow the instructions for your distro; this will require installing the Docker Engine and CLI, as well as [Containerd](https://containerd.io/).
2. [Docker Compose](https://docs.docker.com/compose/install/) - follow the instructions for your operating system to install Compose, this controls the orchestration of the Docker containers which make up the platform.

The pre-requisites for installing Docker are minimal, in general, the most important factor is that your OS is up to date - on Linux systems you will need a relatively up-to-date kernel.

To run the platform the minimum requirements are simply those required to install and run Docker, these can be found in the [Docker documentation](https://docs.docker.com/).

For running in production, we recommend the use of a Linux host. If you run into any issues getting this setup we recommend checking the output of this script - if you are missing some services here you may need to perform a distribution upgrade.

### Setting up&#x20;

Once you have Docker fully installed the rest of the process should be simple! You'll want to download the following files to your host:

1. [docker-compose.yaml](https://raw.githubusercontent.com/Budibase/budibase/master/hosting/docker-compose.yaml) - this defines the images used and how to orchestrate them.
2. [.env](https://raw.githubusercontent.com/Budibase/budibase/master/hosting/.env) - this contains all the default settings for your platform.

> ### 🚧If running in production, update secrets in `.env`
>
> If you are planning to run the cluster in a production environment, you will want update the values in the secrets section of the `.env` file before moving on, so that only you can access the database, file storage and internal APIs etc. Using a new UUID for each secret is a good approach.

Once you have these files downloaded, running the platform is as simple as:

Shell

```shell
docker-compose up
```

Running the above command in a prompt or terminal will start up the cluster, installing the latest images for our required services and setting up a network to run them across - this may take some time to download the necessary resources.

When the cluster is ready you'll see a mixture of log messages from the various services.

You can access your new installation at:

```
<server-ip>:10000
```

If you run into any issues the easiest way to resolve them would be to [raise a discussion in our community](https://github.com/Budibase/budibase/discussions).

#### Updating Docker compose configuration

If you need to make changes to your `docker-compose.yaml` or `.env` files, you'll need to take your containers down and then bring them up again to make the changes apply.

Use `docker compose down` to stop your containers and then `docker compose up -d` to start them with the changes applied.

### Further information

For those out there interested in a more advanced setup you may want to make alternations to the `docker-compose.yaml` config file. Below, we've covered the various services and their purposes:

1. [CouchDB](https://couchdb.apache.org/) - this is our database service, making use of a docker volume to persist data.
2. [Minio](https://min.io/) - an open-source alternative to S3, acting as an object store for files, binary data, attachments and so on.
3. [Nginx](https://www.nginx.com/) - our proxy engine, this provides a single interface by which all services can be communicated with as required.
4. Worker - a background service engine that can handle tasks in the background of the main app servers.
5. Server - the main face of the platform, providing the APIs which your apps will make use of.

All of these services need to be able to communicate with each other, however, they can be hosted separately if desired, as long as they have the correct URLs to reach each other they will function.
