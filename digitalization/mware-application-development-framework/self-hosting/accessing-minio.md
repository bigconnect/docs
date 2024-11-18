# Accessing MinIO

> The following content applies to self-hosted deployments only

Mware Application Development Framework uses a MinIO object store, to store:

* Web front-end files
* Attachments

You have full access to MinIO, via the official client. Before you access the MinIO client, follow these steps:

1. Enable MinIO browsing. Your **docker-compose.yaml** should contain a setting called **MINIO\_BROWSER** - make sure its value is **on**.
2. If you made a change to your **docker-compose.yaml**, you will need to save the file and restart your cluster.
3. Now, find your access key and secret. Your **.env** file should contain these values: **MINIO\_ACCESS\_KEY**, and **MINIO\_SECRET\_KEY**.
4. You will also need to port forward. Your **.env** file should contain the value: **MINIO\_PORT**.

#### Example config

yaml

```yaml
minio-service:
	ports:
  		- "${MINIO_PORT}:9000"
  		- "9001:9001"
	environment:
  		MINIO_ROOT_USER: ${MINIO_ACCESS_KEY}
    	MINIO_ROOT_PASSWORD: ${MINIO_SECRET_KEY}
    	MINIO_BROWSER: "on"
```

Now you should be able to access MinIO via the URL:

```
your-host/minio
```
