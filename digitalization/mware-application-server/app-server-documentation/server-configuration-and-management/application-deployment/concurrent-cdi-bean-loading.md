# Concurrent CDI Bean Loading

It is possible to concurrently load CDI beans and potentially speed up an application’s load time by enabling Weld’s Multi-Threaded Bean Loading feature.

### Enable Concurrent CDI Bean Loading <a href="#enable-concurrent-cdi-bean-loading" id="enable-concurrent-cdi-bean-loading"></a>

Concurrent CDI bean loading is configured on a per-configuration basis using the `set` on the `cdi-service.enable-concurrent-deployment` property like this:

```bash
asadmin set configs.config.server-config.cdi-service.enable-concurrent-deployment=true
```

It is possible to configure the number of pre-loader threads like this:

```bash
asadmin set configs.config.server-config.cdi-service.pre-loader-thread-pool-size=4
```

When configuring the number of the pre-loader threads, use a sensible value that is consistent with the number of CPU cores available for the server instance, otherwise this setting may introduce performance degradation on the deployment times.
