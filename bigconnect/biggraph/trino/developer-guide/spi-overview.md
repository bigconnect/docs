# SPI overview

When you implement a new Trino plugin, you implement interfaces and override methods defined by the Service Provider Interface (SPI).

Plugins can provide additional:

* Connectors,
* block encodings,
* Types,
* Functions,
* System access control,
* Group provider,
* Password authenticator,
* Header authenticator,
* Certificate authenticator,
* Event listener,
* resource group configuration managers,
* session property configuration managers,
* and exchange managers.

In particular, connectors are the source of all data for queries in Trino: they back each catalog available to Trino.

### Code[#](https://trino.io/docs/current/develop/spi-overview.html#code) <a href="#code" id="code"></a>

The SPI source can be found in the `core/trino-spi` directory in the Trino source tree.

### Plugin metadata[#](https://trino.io/docs/current/develop/spi-overview.html#plugin-metadata) <a href="#plugin-metadata" id="plugin-metadata"></a>

Each plugin identifies an entry point: an implementation of the `Plugin` interface. This class name is provided to Trino via the standard Java `ServiceLoader` interface: the classpath contains a resource file named `io.trino.spi.Plugin` in the `META-INF/services` directory. The content of this file is a single line listing the name of the plugin class:

```
com.example.plugin.ExamplePlugin
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

For a built-in plugin that is included in the Trino source code, this resource file is created whenever the `pom.xml` file of a plugin contains the following line:

```
<packaging>trino-plugin</packaging>
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

### Plugin[#](https://trino.io/docs/current/develop/spi-overview.html#plugin) <a href="#plugin" id="plugin"></a>

The `Plugin` interface is a good starting place for developers looking to understand the Trino SPI. It contains access methods to retrieve various classes that a Plugin can provide. For example, the `getConnectorFactories()` method is a top-level function that Trino calls to retrieve a `ConnectorFactory` when Trino is ready to create an instance of a connector to back a catalog. There are similar methods for `Type`, `ParametricType`, `Function`, `SystemAccessControl`, and `EventListenerFactory` objects.

### Building plugins via Maven[#](https://trino.io/docs/current/develop/spi-overview.html#building-plugins-via-maven) <a href="#building-plugins-via-maven" id="building-plugins-via-maven"></a>

Plugins depend on the SPI from Trino:

```
<dependency>
    <groupId>io.trino</groupId>
    <artifactId>trino-spi</artifactId>
    <scope>provided</scope>
</dependency>
```

![Copy to clipboard](https://trino.io/docs/current/\_static/copy-button.svg)

The plugin uses the Maven `provided` scope because Trino provides the classes from the SPI at runtime and thus the plugin should not include them in the plugin assembly.

There are a few other dependencies that are provided by Trino, including Slice and Jackson annotations. In particular, Jackson is used for serializing connector handles and thus plugins must use the annotations version provided by Trino.

All other dependencies are based on what the plugin needs for its own implementation. Plugins are loaded in a separate class loader to provide isolation and to allow plugins to use a different version of a library that Trino uses internally.

For an example `pom.xml` file, see the example HTTP connector in the `plugin/trino-example-http` directory in the Trino source tree.

### Deploying a custom plugin[#](https://trino.io/docs/current/develop/spi-overview.html#deploying-a-custom-plugin) <a href="#deploying-a-custom-plugin" id="deploying-a-custom-plugin"></a>

Because Trino plugins use the `trino-plugin` packaging type, building a plugin will create a ZIP file in the `target` directory. This file contains the plugin JAR and all its dependencies JAR files.

In order to add a custom plugin to a Trino installation, extract the plugin ZIP file and move the extracted directory into the Trino plugin directory. For example, for a plugin called `my-functions`, with a version of 1.0, you would extract `my-functions-1.0.zip` and then move `my-functions-1.0` to `my-functions` in the Trino plugin directory.

Note

Every Trino plugin should be in a separate directory. Do not put JAR files directly into the `plugin` directory. Plugins should only contain JAR files, so any subdirectories will not be traversed and will be ignored.

By default, the plugin directory is the `plugin` directory relative to the directory in which Trino is installed, but it is configurable using the configuration variable `plugin.dir`. In order for Trino to pick up the new plugin, you must restart Trino.

Plugins must be installed on all nodes in the Trino cluster (coordinator and workers).
