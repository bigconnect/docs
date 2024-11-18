# Updating

There are several ways to update your installation, based on your chosen setup.

### CLI (Docker compose)

If you set up Mware Application Development Framework using the CLI Install, you can update your installation directly by navigating to the directory where you installed it (with your `docker-compose.yaml`, `.env` file etc) and run the following CLI commands.

🎉Update using CLIOpen Recipe

### Standard Docker Upgrade

If you prefer, you can also just use standard docker commands to upgrade your installation.

#### Upgrading a Docker Single Image Installation

First, stop and remove your container with:

Shell

```shell
docker stop <container-id>
docker rm <container-id>
```

Then run the following to get the latest version of the container:

Shell

```shell
docker pull ...
```

With the latest version of your container, you can now run the same `docker run` command that you ran to set it up in the first place.

> ### 📘Note
>
> Tools like [https://www.portainer.io/](https://www.portainer.io/) and [https://github.com/containrrr/watchtower](https://github.com/containrrr/watchtower) can be a powerful way to simplify and automate container management for you.
>
> Another option is to migrate your single image installation to using docker-compose. Which will let you take advantage of the next method below.

#### Upgrading a Docker Compose Installation

First, navigate to the directory where you set up your installation with docker compose. It should contain:

* `docker-compose.yaml`
* `.env`

You can then run the following commands. (You may need to use `docker-compose` on older versions of docker)

Shell

```shell
docker compose pull 
docker compose up -d
```

#### Downgrading a Docker Compose Installation

To downgrade, if you are using a docker compose, you can go to each image line of the docker-compose.yaml file and type down the desired version after a colon. For example:

docker-compose.yaml

```yaml
image: ....docker.scarf.sh/.../apps:2.17.6
```

Do this for all images except minio, watchtower and couchdb. The container version matches the app version.

Once you have updated your docker-compose.yaml file, you will need to bring down your containers and then start them up again.

Shell

```shell
docker compose down
docker compose up -d
```

### Kubernetes

Instructions on how to upgrade running on Kubernetes can be found on Kubernetes
