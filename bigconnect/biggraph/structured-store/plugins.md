# Plugins

The Ignite plugin system allows you to extend the core functionality of Ignite. Plugins have access to different internal Ignite components, such as security processor and others, and can extend the programmatic API of Ignite.

To add a custom plugin, implement the `PluginProvider` interface and register the implementation in the node configuration. The following is an overview of the steps involved in creating a plugin:

1. Implement the `PluginProvider` interface. This is the main interface for creating plugins.
2. Implement the `IgnitePlugin` interface. If your plugin adds functionality that is meant to be triggered by end users, you should add public methods to this class. An instance of this class is available to end users at runtime via `Ignite.plugin(String pluginName)`.
3. Register the plugin in `IgniteConfiguration.setPluginProviders(…​)` either programmatically or via XML configuration.
4. If your plugin has a public API, call `MyPlugin plugin = Ignite.plugin(pluginName)` at runtime and execute specific actions.

The following section gives an example of a plugin and goes into details about how plugins work in Ignite.
