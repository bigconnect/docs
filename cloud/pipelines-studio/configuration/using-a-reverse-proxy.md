# Using a Reverse Proxy

You can use BigConnect Pipelines with a reverse proxy server, such as Nginx. To use a reverse proxy server, you simply configure the reverse proxy server to work with BigConnect Pipelines.

You might use a reverse proxy server to define user-defined path-based routes to BigConnect Pipelines or to help balance loads between multiple instances of BigConnect Pipelines installed on the same machine.

You can also use a reverse proxy server for BigConnect Pipelines Transformer.

For example, the following Nginx configuration provides access to two instances of BigConnect Pipelines, one using port 18630, and one using 18640. It also provides access to BigConnect Pipelines Transformer on port 19630:

```
location /sdc/ {
    proxy_pass         http://0.0.0.0:18630/;
    proxy_set_header   Host $host:$server_port;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection "Upgrade";
}

location /sdc2/ {
    proxy_pass         http://0.0.0.0:18640/;
    proxy_set_header   Host $host:$server_port;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection "Upgrade";
}

location /transformer/ {
    proxy_pass         http://0.0.0.0:19630/;
    proxy_set_header   Host $host:$server_port;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection "Upgrade";
}
```

Similarly, to use an Nginx ingress controller to access BigConnect Pipelines configured to use port 18630 on a Kubernetes cluster, you might use the following configuration:

```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: streamsets-datacollector
  annotations:
    nginx.ingress.kubernetes.io/ssl-redirect: \"false\"
    nginx.ingress.kubernetes.io/rewrite-target: /$2
spec:
  rules:
    - http:
        paths:
          - path: /datacollector(/|$)(.*)
            backend:
              serviceName: streamsets-datacollector
              servicePort: 18630
```

For specific details on configuring a reverse proxy server, see your reverse proxy server documentation.
