# Observability with OpenTelemetry

Trino exposes tracing information for observability of a running Trino deployment for the widely used OpenTelemetry collection of APIs, SDKs, and tools. You can use OpenTelemetry to instrument, generate, collect, and export telemetry data such as metrics, logs, and traces to help you analyze application performance and behavior. More information about the observability and the concepts involved is available in the OpenTelemetry documentation.

The integration of OpenTelemetry with Trino enables tracing Trino behavior and performance. You can use it to diagnose the overall application as well as processing of specific queries or other narrower aspects.

Trino emits trace information from the coordinator and the workers. Trace information includes the core system such as the query planner and the optimizer, and a wide range of connectors and other plugins.

Trino uses any supplied trace identifiers from client tools across the cluster. If none are supplied, trace identifiers are created for each query. The identifiers are propagated to data sources, metastores, and other connected components. As a result you can use this distributed tracing information to follow all the processing flow of a query from a client tool, through the coordinator and all workers to the data sources and other integrations.

If you want to receive traces from data sources and other integrations, these tools must also support OpenTelemetry tracing and use the supplied identifiers from Trino to propagate the context. Tracing must be enabled separately on these tools.

### Configuration[#](https://trino.io/docs/current/admin/opentelemetry.html#configuration) <a href="#configuration" id="configuration"></a>

Use tracing with OpenTelemetry by enabling it and configuring the endpoint in the config.properties file:

```
tracing.enabled=true
tracing.exporter.endpoint=http://observe.example.com:4317
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Tracing is not enabled by default. The exporter endpoint must specify a URL that is accessible from the coordinator and all workers of the cluster. The preceding example uses a observability platform deployment available by HTTP at the host `observe.example.com`, port `4317`.

### Example use[#](https://trino.io/docs/current/admin/opentelemetry.html#example-use) <a href="#example-use" id="example-use"></a>

The following steps provide a simple demo setup to run the open source observability platform [Jaeger](https://www.jaegertracing.io/) and Trino locally in Docker containers.

Create a shared network for both servers called `platform`:

```
docker network create platform
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Start Jaeger in the background:

```
docker run -d \
  --name jaeger \
  --network=platform \
  --network-alias=jaeger \
  -e COLLECTOR_OTLP_ENABLED=true \
  -p 16686:16686 \
  -p 4317:4317 \
  jaegertracing/all-in-one:latest
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

The preceding command adds Jaeger to the `platform` network with the hostname `jaeger`. It also maps the endpoint and Jaeger UI ports.

Create a `config.properties` file that uses the default setup from the Trino container, and adds the tracing configuration with the `jaeger` hostname:

```
node-scheduler.include-coordinator=true
http-server.http.port=8080
discovery.uri=http://localhost:8080
tracing.enabled=true
tracing.exporter.endpoint=http://jaeger:4317
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Start Trino in the background:

```
docker run -d \
  --name trino \
  --network=platform \
  -p 8080:8080 \
  --mount type=bind,source=$PWD/config.properties,target=/etc/trino/config.properties \
  trinodb/trino:latest
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

The preceding command adds Trino to the `platform` network. It also mounts the configuration file into the container so that tracing is enabled.

Now everything is running.

Install and run the Trino CLI or any other client application and submit a query such as `SHOW CATALOGS;` or `SELECT * FROM tpch.tiny.nation;`.

Optionally, log into the Trino Web UI at http://localhost:8080 with a random username. Press the **Finished** button and inspect the details for the completed queries.

Access the Jaeger UI at http://localhost:16686/, select the service `trino`, and press **Find traces**.

As a next step, run more queries and inspect more traces with the Jaeger UI.

Once you are done you can stop the containers:

```
docker stop trino
docker stop jaeger
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

You can start them again for further testing:

```
docker start jaeger
docker start trino
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

Use the following commands to completely remove the network and containers:

```
docker rm trino
docker rm jaeger
docker network rm platform
```
