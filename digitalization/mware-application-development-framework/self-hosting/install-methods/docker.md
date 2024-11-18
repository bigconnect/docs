# Docker

Within this guide, you will learn how to deploy Mware Application Development Framework using the all-in-one Docker image.

The first step to using our Docker image is to make sure your host has a recent version of [Docker](https://www.docker.com/) installed. The image can be found on [Docker hub](https://hub.docker.com/r/budibase/budibase).

There are two methods for running the image, these are detailed below.

### Method 1 - Docker run

The easiest way to try out the image is with a docker run command, the most basic version of this being:

Shell

```shell
docker run -d -t \
  --name= name \
  -p 10000:80 \
  -v /local/path/data:/data \
  --restart unless-stopped \
  /name:latest
```

This command will start a container on your system, which you can then connect to on [<<\<http://localhost:10000>>>](http://localhost:10000/). For this to work you will also need to specify an absolute path to mount as a volume for the container, replacing the `/local/path/data` component of the command.

There are a few other parts of this command we can change, as well as a range of environment variables that should be set before using this in a production environment.

* The port which it can be accessed on can be changed by altering the `-p 10000:80` component, changing from port 10000 to whatever else is desired.
* The following environment variables should be set before putting this container into production (using the `-e <variable>` syntax).

| Variable name                             | Description                                                                                                                                                                                                                             |
| ----------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `CUSTOM_DOMAIN`                           | If a domain in the format of `domain.com` is set, then system will automatically attempt to create an SSL certificate and use HTTPS for this domain. The domain must already be directed to the container for this to verify correctly. |
| `INTERNAL_API_KEY`                        | An API key which can be used to access many core components, this should be updated to some random string.                                                                                                                              |
| `JWT_SECRET`                              | A secret key used to secure all sessions, this should be updated to a random string. Please note that changing this string will invalidate all existing sessions.                                                                       |
| `MINIO_ACCESS_KEY` and `MINIO_SECRET_KEY` | These two environment variables should be set to some combination of random strings to secure access to MinIO. These can be used to login in to the MinIO browser if desired.                                                           |
| `REDIS_PASSWORD`                          | The password used to access the Redis instance, this should be set to a random string to guarantee security.                                                                                                                            |
| `COUCHDB_USER` and `COUCHDB_PASSWORD`     | These two environment variables define the CouchDB username and password used to access the main admin user. These can also be used to access the Fauxton user interface.                                                               |

These environment variables will automatically be pre-seeded with UUIDs if no environment variable is set with the pre-seeded values being accessible in the `.env` file located in the mounted volume. If you wish to update any values then you will need to update them within the `.env` file.

### Method 2 - Docker compose

On some hosts will may be able to utilise Docker compose to simplify the configuration process, this will allow setting environment variables and easily controlling the mounted volume. Below we've provided an example `docker-compose.yml` that can be used in conjunction with the command `docker-compose up -d`.

YAML

```yaml
version: "3"

services:
  name:
    restart: unless-stopped
    image: /name:latest
    ports:
      - "8080:80"
    environment:
      JWT_SECRET: <secret>
      MINIO_ACCESS_KEY: <secret>
      MINIO_SECRET_KEY: <secret>
      REDIS_PASSWORD: <secret>
      COUCHDB_USER: <secret>
      COUCHDB_PASSWORD: <secret>
      INTERNAL_API_KEY: <secret>
    volumes:
      - name_data:/data

volumes:
  name_data:
    driver: local
```

This makes it easier to manage your image, including its configuration and volumes - all that is needed to utilise the configuration is replacing the `<secret>` options with secrets you have defined.

#### Importing a CA certificate

If you need to use a custom certificate file in your self-hosted instance, you will need to add an environment variable to both the **server** and **worker** containers.

### Recommended resources

The number of processing cores and amount of RAM required by docker containers will depend greatly on expected usage, however for most use cases **1 core and 2GB of RAM** should be enough.
