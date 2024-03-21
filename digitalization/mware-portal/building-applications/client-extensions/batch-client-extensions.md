# Batch Client Extensions

Batch client extensions work with the portal’s batch engine framework to provide data entities to your portal instance. Export one instance’s data (e.g., via `curl` or the Export/Import Center). Then add it to a batch client extension and deploy it to bring the data into a different portal instance.

To export data to use with batch client extensions, you can use the Export/Import Center via the Applications Menu ( ![Applications menu icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png) ). To enable the Export/Import center, add this portal property before starting the portal:

```properties
feature.flag.COMMERCE-8087=true
```

WARNING

The batch framework’s export/import center is a feature under development. Never enable a dev feature flag in production.

Specify the batch client extension in your `client-extension.yaml` file:

```yaml
type: batch
```

You can export any type of data supported by the batch engine framework from your portal instance, including object definitions and workflow definitions.

IMPORTANT

When exporting, use the `jsont` file format. `jsont` is required for `*.batch-engine-dat.json` files when using in conjunction with batch client extensions.

Once the `*.batch-engine-data.json` file is added to a batch client extension, deploy the built archive to Cloud or self-hosted portal instances.

\
