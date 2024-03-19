# Batch Client Extensions

Batch client extensions work with Liferay’s [batch engine framework](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/consuming-apis/batch-engine-api-basics-exporting-data) to provide data entities to your Liferay instance. Export one instance’s data (e.g., via `curl` or the Export/Import Center). Then add it to a batch client extension and [deploy](https://learn.liferay.com/web/guest/w/dxp/building-applications/client-extensions/working-with-client-extensions#deploying-to-your-liferay-instance) it to bring the data into a different Liferay instance.

To export data to use with batch client extensions, you can use the Export/Import Center via the Applications Menu ( ![Applications menu icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png) ). To enable the Export/Import center, add this [portal property](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/reference/portal-properties) before starting Liferay:

```properties
feature.flag.COMMERCE-8087=true
```

WARNING

The batch framework’s export/import center is a [feature under development](https://learn.liferay.com/web/guest/w/dxp/system-administration/configuring-liferay/feature-flags#dev-feature-flags). Never enable a dev feature flag in production.

Specify the batch client extension in your `client-extension.yaml` file:

```yaml
type: batch
```

You can export any type of data supported by the [batch engine framework](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/consuming-apis/batch-engine-api-basics-exporting-data) from your Liferay instance, including [object definitions](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects) and [workflow definitions](https://learn.liferay.com/web/guest/w/dxp/process-automation/workflow/introduction-to-workflow).

IMPORTANT

When exporting, use the `jsont` file format. `jsont` is required for `*.batch-engine-dat.json` files when using in conjunction with batch client extensions.

Once the `*.batch-engine-data.json` file is added to a [batch client extension](https://learn.liferay.com/web/guest/w/dxp/building-applications/client-extensions/batch-client-extensions/batch-yaml-configuration-reference), deploy the [built archive](https://learn.liferay.com/web/guest/w/dxp/building-applications/client-extensions/packaging-client-extensions) to Liferay Cloud or self-hosted Liferay instances.

\
