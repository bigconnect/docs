# Accessing CouchDB

> The following content applies to self-hosted deployments only

Mware Application Development Framework uses CouchDB to store:

* Metadata about your apps and deployment
* Data stored when using it's in-built tables
* Audit logs

You have full access to CouchDb, via [Fauxton](https://couchdb.apache.org/fauxton-visual-guide/index.html) - the official CouchDB client. You may access Fauxton via the URL:

```
your-host/db/_utils/#
```

However, you will need a username and password to log in. You may find these in your **.env** file, which lives in the same directory as your **docker-compose.yaml**.

Also make sure you are port forwarding to 5984.

Use the following values from your .env file:

* COUCH\_DB\_USER
* COUCH\_DB\_PASSWORD
* COUCH\_DB\_PORT

#### Example config

YAML

```yaml
couchdb-service:
    ports: 
      - "${COUCH_DB_PORT}:5984"
    environment:
      - COUCHDB_PASSWORD=${COUCH_DB_PASSWORD}
      - COUCHDB_USER=${COUCH_DB_USER}
      
```
