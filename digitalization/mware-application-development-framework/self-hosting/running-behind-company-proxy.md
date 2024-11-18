# Running behind company proxy

If you are running Mware Application Development Framework in an environment where you need to use a proxy, such as inside an organizational network you can have proxy all it's backend requests through a proxy of your choice. This functionality is driven by the `global-agent` package:

[https://github.com/gajus/global-agent](https://github.com/gajus/global-agent)

#### Configuration

There are 2 environment variables you will need to run it behind a company proxy:

| Variable                   | Purpose                                                                          | Required |
| -------------------------- | -------------------------------------------------------------------------------- | -------- |
| `GLOBAL_AGENT_HTTP_PROXY`  | Where to proxy all server HTTP requests through                                  | Yes      |
| `GLOBAL_AGENT_HTTPS_PROXY` | Where to proxy all server HTTPS requests through                                 | No       |
| `GLOBAL_AGENT_NO_PROXY`    | A pattern of URLs that should be excluded from proxying. Eg. `*.foo.com,baz.com` | No       |

Bear in mind that these variables need to be made available to both the `worker` and `app` services, so if using docker compose, you must add them to your docker compose configuration:

YAML

```yaml
services:
  app-service:
    restart: unless-stopped
    image: ....docker.scarf.sh/.../apps
    container_name: bbapps
    environment:
     GLOBAL_AGENT_HTTP_PROXY: http://my-proxy.net
     GLOBAL_AGENT_HTTPS_PROXY: https://my-proxy.net
  worker-service:
    restart: unless-stopped
    image: ....docker.scarf.sh/.../apps
    container_name: bbworker
    environment:
      GLOBAL_AGENT_HTTP_PROXY: http://my-proxy.net
      GLOBAL_AGENT_HTTPS_PROXY: https://my-proxy.net
```

For container to container communication using `docker-compose` - you may need to add the following to avoid that local communication going through your proxy:

`GLOBAL_AGENT_NO_PROXY: couchdb-service,app-service,worker-service,minio-service,redis-service,localhost`

For single image setups, you can pass these environment variables using standard `docker run` syntax.

Bash

```shell
docker run -d -t 
-e GLOBAL_AGENT_HTTP_PROXY=http://my-proxy.net 
-p 10000:80 
-v /local/path/data:/data  
--restart unless-stopped 
.../....:latest
```

#### Outbound Allow-list

To use your deployment, you must allow a list of outbound connections. These are as follows:

| URL                                                                 | Purpose |
| ------------------------------------------------------------------- | ------- |
| [https://cdn.jsdelivr.net](https://cdn.jsdelivr.net/)               | Fonts   |
| [https://fonts.gstatic.com](https://fonts.gstatic.com/)             | Fonts   |
| [https://rsms.me](https://rsms.me/)                                 | Fonts   |
| [https://maxcdn.bootstrapcdn.com](https://maxcdn.bootstrapcdn.com/) | Fonts   |
