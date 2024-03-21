# Packaging Client Extensions

Client extension projects are built as deployable archives called _Universal File Format Archives_ (LUFFAs) with a `.zip` extension. Each LUFFA has a particular structure and defines specific component files, which are generated automatically if not provided by your project. Liferay workspace packages client extension projects when you build them. Understanding how packaging works is important for assembling client extensions or creating LUFFAs with a different process.

This is a LUFFA’s structure:

```bash
.
├── batch
│   └── **/*.batch-engine-data.json
├── *.client-extension-config.json
├── Dockerfile
├── LCP.json
├── static
│   └── **/*
├── WEB-INF
│   └── liferay-plugin-package.properties
└── [microservice resources]
```

### `batch` <a href="#batch" id="batch"></a>

NOTE

The `batch` folder is only required by batch client extension projects.

The data entities in a batch client extension are `*.batch-engine-data.json` files inside a `batch/` folder in the built LUFFA. These files can be in any folder structure within the `batch/` folder.

You can place any number of `*.batch-engine-data.json` files under the root level `batch/` directory, using a directory structure of unlimited depth. If you fail to define a batch extension in your project’s `client-extension.yaml` file, Liferay ignores these files.

### `*.client-extension-config.json` <a href="#client-extension-configjson" id="client-extension-configjson"></a>

NOTE

At least one `*.client-extension-config.json` file is required in each LUFFA.

At the root of the LUFFA are one or more `*.client-extension-config.json` ([OSGi Configuration Resource Format](https://docs.osgi.org/specification/osgi.cmpn/7.0.0/service.configurator.html#d0e131566)) files. They define how your client extensions are structured in the archive. Usually these files are produced during the build process based on your `client-extension.yaml` file’s contents. The conversion from YAML to JSON is specified by the workspace plugin, and is not a 1:1 mapping.

### `Dockerfile` <a href="#dockerfile" id="dockerfile"></a>

NOTE

At least one Dockerfile is required in each LUFFA.

The build process automatically generates and packages a `Dockerfile` for batch, configuration, and front-end client extensions.

You must provide a `Dockerfile` in your project for microservice client extensions. Add it to the root of your project, and it’s copied into the LUFFA when you build the project. Make sure your `Dockerfile` can execute the microservice client extensions in your project. For example, your `Dockerfile` may need to install specific tools that your microservice’s code needs to run.

#### Packaging a Client Extension Manually <a href="#packaging-a-client-extension-manually" id="packaging-a-client-extension-manually"></a>

If packaging the LUFFA yourself, batch, configuration, and front-end client extensions requires certain `Dockerfile` conventions. The portal provides them for you in the images below.

For batch client extensions, use the `liferay/batch:latest` image:

```dockerfile
FROM liferay/batch:latest
COPY /batch /batch
```

For configuration client extensions, use the `liferay/noop:latest` image:

```dockerfile
FROM liferay/noop:latest
```

For front-end client extensions, use the `liferay/caddy:latest` image:

```dockerfile
FROM liferay/caddy:latest
COPY static/ /public_html/
```

A microservice client extension’s `Dockerfile` depends entirely on your specific context, so a pre-configured image cannot be provided.

### `LCP.json` <a href="#lcpjson" id="lcpjson"></a>

NOTE

An `LCP.json` file is required in each LUFFA.

The build process automatically generates and packages an `LCP.json` file for batch, configuration, and front-end.

You must provide an `LCP.json` file in your project for microservice client extensions. Add it to the root of your project, and it’s copied into the LUFFA when your project is built. This `LCP.json` file configures the container used for the microservice when it’s deployed in Cloud.

If packaging the LUFFA yourself, each client extension comes with different specification suggestions for its `LCP.json`:

| Client Extension                                                                                                                                                               | Requires Significant Resources | Kind       | Notes                                                                                                                                                                                                                                                                                                                                                  |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------ | ---------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| [Batch](https://learn.liferay.com/web/guest/w/dxp/building-applications/client-extensions/packaging-client-extensions#example-batch-client-extension-lcp-json)                 | ✘                              | Job        | <ul><li>The environment variable <code>LIFERAY_BATCH_OAUTH_APP_ERC</code> must be set to the value of your batch client extension’s <code>oAuthApplicationHeadlessServer</code> property. This can be provided through interpolation.</li><li>You can specify small values for <code>cpu</code>, <code>memory</code> and <code>scale</code>.</li></ul> |
| [Configuration](https://learn.liferay.com/web/guest/w/dxp/building-applications/client-extensions/packaging-client-extensions#example-configuration-client-extension-lcp-json) | ✘                              | Job        | <ul><li>Even less memory intensive than batch extensions.</li><li>You can specify very small values for <code>cpu</code>, <code>memory</code> and <code>scale</code>.</li></ul>                                                                                                                                                                        |
| [Front-End](https://learn.liferay.com/web/guest/w/dxp/building-applications/client-extensions/packaging-client-extensions#example-front-end-client-extension-lcp-json)         | ✘                              | Deployment | <ul><li>Must specify the <code>loadBalancer</code> property with <code>targetPort</code> set to <code>80</code>.</li><li>Should specify <code>livenessProbe</code> and <code>readinessProbe</code> properties for self-healing.</li><li>You can specify small values for <code>cpu</code>, <code>memory</code> and <code>scale</code>.</li></ul>       |

#### Example Batch Client Extension LCP.json <a href="#example-batch-client-extension-lcpjson" id="example-batch-client-extension-lcpjson"></a>

```json
{
   "cpu": 0.2,
   "env": {
       "LIFERAY_BATCH_OAUTH_APP_ERC": "__batch.oAuthApplicationHeadlessServer__"
   },
   "id": "__CLIENT_EXTENSION_ID__",
   "kind": "Job",
   "memory": 50,
   "scale": 1
}
```

#### Example Configuration Client Extension LCP.json <a href="#example-configuration-client-extension-lcpjson" id="example-configuration-client-extension-lcpjson"></a>

```json
{
    "cpu": 0.1,
    "id": "__CLIENT_EXTENSION_ID__",
    "kind": "Job",
    "memory": 10,
    "scale": 1
}
```

#### Example Front-End Client Extension LCP.json <a href="#example-front-end-client-extension-lcpjson" id="example-front-end-client-extension-lcpjson"></a>

```json
{
    "cpu": 0.2,
    "id": "__CLIENT_EXTENSION_ID__",
    "kind": "Deployment",
    "livenessProbe": {
        "httpGet": {
            "path": "/",
            "port": 80
        }
    },
    "loadBalancer": {
        "cdn": true,
        "targetPort": 80
    },
    "memory": 50,
    "readinessProbe": {
        "httpGet": {
            "path": "/",
            "port": 80
        }
    },
    "scale": 1
}
```

See Configuration via LCP.json for more information.

### `static` <a href="#static" id="static"></a>

NOTE

The `static` directory is only required by front-end client extension projects.

You can place any number of static resource files into the root level `static/` directory in the built LUFFA. Use any folder structure you like within the `static/` folder. These files are ignored if there is no front-end client extension defined in your project’s `client-extension.yaml` file.

### Microservice Resources <a href="#microservice-resources" id="microservice-resources"></a>

You can include additional resources in the built LUFFA for your microservice client extension projects, as long as they do not conflict with other required files.

\
