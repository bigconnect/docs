# Migrations

Occasionally as part of an update we will need to make an update to the infrastructure which is used by the platform - in self host this can impact you.

The system will warn you that your installation is not up to date/is missing components of the infrastructure, this warning will look like:

Below is a list of migrations which may be needed for your installation.

### DB SQL Support

If you have reached this page then you have been alerted to an issue with your self-host installation. The system relies on a service called the Structured Query Server ([SQS](https://neighbourhood.ie/products-and-services/structured-query-server)). This runs alongside CouchDB to provide relational searching functionality.

Below is what's required for each installation method to get SQS running.

#### Single image

If you are using any of the single image/docker install methods then you will not need to make any changes, the updates to CouchDB are included in the Docker image.

#### `docker-compose`

You need to update your `docker-compose.yaml` file - the quickest way to do this is using the CLI. If you would prefer to do this manually then you will need to make sure your CouchDB installation is using the latest version of the `/couchdb` image. This image runs SQS for you.

You will also need to set a new environment variable called `TARGETBUILD`. Here's how your `couchdb-service` block in your `docker-compose.yaml` file should look:

docker-compose.yaml

```yaml
couchdb-service:
  restart: unless-stopped
  image: .../couchdb:v3.3.3-sqs-v2.1.1
  environment:
    - COUCHDB_PASSWORD=${COUCH_DB_PASSWORD}
    - COUCHDB_USER=${COUCH_DB_USER}
    - TARGETBUILD=docker-compose
  volumes:
    - couchdb3_data:/opt/couchdb/data
```

#### Kubernetes

You will need to make sure you are using the latest version of the Helm chart alongside the latest version of the Budibase images. **If your upgrade workflow for Kubernetes involves just updating image tags in your `values.yaml` file, this will break your installation.**

If you followed the steps in Kubernetes, you'll need to run the following commands to update your Helm chart.

```
helm repo update ...
helm upgrade -n ...
```

***

If you are still having issues and have confirmed that your installation is running the latest `.../couchdb` image for your CouchDB service then please reach out to our support for further help, in the meantime you can downgrade to the previously used version safely.
