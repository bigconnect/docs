# Developer Guide

## REST APIs

BigConnect Explorer is a webapp and it has specific endpoints that you can use to develop your own REST clients.

There is an REST API browser included in the Web Console that you can explore and play with the different API endpoints. You can find it in the admin console, under the REST API section.

![BigConnect Eplorer REST API](<../../../.gitbook/assets/image (695).png>)

## Web Framework

BigConnect Explorer is designed to be a single page application, but requests must be made back to the server to retrieve information, process data, and manage many other back-end services. Since BigConnect Explorer was designed to be extensible with many different types of plugins, web plugins fit naturally into BigConnect Explorer's architecture.

The Explorer has its own minimalistic, lightning-fast web framework. It provides a thin layer of fundamental web application features without adding much overhead.

Web plugins are deployed alongside Explorer inside of the web server and are used to both add custom functionality and override existing components.

The framework provides routing and parameter binding. Routing refers to determining how an application responds to a client request to a particular endpoint, which is a URI (or path) and a specific HTTP request method (GET, POST, and so on). Each route can have one or more handler functions, which are executed when the route is matched.

Route definition takes the following structure: `app.METHOD (path, handlers)` where:

* `app` in an instance of [com.mware.web.framework.App](https://github.com/bigconnect/bigconnect-explorer/blob/master/web-base/src/main/java/com/mware/web/framework/App.java)
* METHOD is an [HTTP request method](https://en.wikipedia.org/wiki/Hypertext\_Transfer\_Protocol#Request\_methods), in lowercase.
* `path` is a path on the server
* `handlers` are a list of classes to be called when the route is matched. Each class must implement the [com.mware.web.framework.Handler](https://github.com/bigconnect/bigconnect-explorer/blob/master/web-base/src/main/java/com/mware/web/framework/Handler.java) or its sub-interfaces, like [com.mware.web.framework.ParameterizedHandler](https://github.com/bigconnect/bigconnect-explorer/blob/master/web-base/src/main/java/com/mware/web/framework/ParameterizedHandler.java)

Route handlers need to satisfy one requirement: they need to have a method annotated with the [Handle](https://github.com/bigconnect/bigconnect-explorer/blob/master/web-base/src/main/java/com/mware/web/framework/annotations/Handle.java) annotation. This method will be called by the framework once the route matches.

Handler methods can also have parameters that will be bound automatically to request parameters, based on their type or name. There are also two annotations available to bind request parameters

* [Optional](https://github.com/bigconnect/bigconnect-explorer/blob/master/web-base/src/main/java/com/mware/web/framework/annotations/Optional.java) - the parameter is optional
* [Required](https://github.com/bigconnect/bigconnect-explorer/blob/master/web-base/src/main/java/com/mware/web/framework/annotations/Required.java) - the parameter is required and the framework will throw an exception if it's not set in the incoming request

## Web Plugins

Creating a web app plugin can range from creating some JavaScript that can execute inside of Explorer all the way to overriding the current functionality of an endpoint inside Explorer. This means that web app plugins will always have some sort of mix of front-end and back-end components that provide functionality.

To start working with a web plugin, you must implement the interface [com.mware.web.WebAppPlugin](https://github.com/bigconnect/bigconnect-explorer/blob/master/web-base/src/main/java/com/mware/web/WebAppPlugin.java) and register all of your helper files in there.&#x20;

To ensure that your web plugin is deployed, it needs to be loaded onto the classpath by adding it's fully qualified class name into the `META-INF/services/com.mware.web.WebAppPlugin` file in a jar that is on the classpath.

Web Plugins can be developed using plain JavaScript or React, depending on your preference:

* Creating a standard plugin
* Creating a React plugin

BigConnect Explorer provides a rich set of UI extension points to develop or alter existing functionality, without altering the main code. &#x20;

## Extension Points

Extension points are places built into BigConnect's Web Console to define additional behavior. Custom plugins can also define extension points that can be implemented by other plugins.

An extension point is simply a mapping from a string – the namespaced extension point name – to a JavaScript object. What kind of object is defined by the consumer of the extension point.

All registered extension points are viewable in the admin panel, under `UI Extensions`, and in the table of contents in this document.

To register some custom behavior, require the [`public/v1/api`](https://docs.bigconnect.io/\~/drafts/-LQEgexO3YP4CDrrELDr/primary/javascript/module-public\_v1\_api.html) module, and use the [`registry`](https://docs.bigconnect.io/\~/drafts/-LQEgexO3YP4CDrrELDr/primary/javascript/module-registry.html) member.

```javascript
require(['public/v1/api'], function(bc) {    var registry = bc.registry;    registry.registerExtension([extension point name], [extension point object])})
```

For example, to add an item to the menu bar, use the Menu bar extension point:

```javascript
bc.registry.registerExtension('org.bigconnect.menubar', {    title: i18n('org.bigconnect.examples.menubar.title'),    identifier: 'org-bigconnect-examples-menubar',    action: {        type: 'pane',        componentPath: 'org/bigconnect/examples/menubar/Pane'    },    welcomeTemplatePath: 'hbs!org/bigconnect/examples/menubar/welcome',    icon: '../img/glyphicons/white/glyphicons_066_tags@2x.png',    options: {        placementHint: 'top',        placementHintAfter: 'search',    }});
```

Web plugins can also define their own extension points. They do not need to be defined ahead of time, simply ask the registry for all registered extensions using a unique point name:

```javascript
registry.extensionsForPoint([extension point name]);// Returns array of registered objects
```

It is good practice to define some documentation for your new extension point. Documenting provides a validation function, and a description shown in the admin panel under UI Extensions. Document the extension point using `registry.documentExtensionPoint` before prior to requesting `registry.extensionsForPoint`

```javascript
registry.documentExtensionPoint('com.example.point',    'Description...',    function () { return true; },    'http://example.com/docs');
```

If you call `documentExtensionPoint` before `extensionsForPoint` all the extensions returned are guaranteed to have passed validation. Invalid extensions are logged as warnings in the JavaScript console. This documentation appears in the admin pane under UI Extensions. Add an external documentation URL using an optional 4th parameter.

nvalid extensions are logged as warnings in the browser JavaScript console.

Please see the list of available extension points with descriptions and code examples under the section Extension Point Reference.\






