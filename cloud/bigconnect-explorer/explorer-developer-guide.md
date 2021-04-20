# Explorer Developer Guide

## REST APIs

BigConnect Explorer is a webapp and it has specific endpoints that you can use to develop your own REST clients.

There is an REST API browser included in the Web Console that you can explore and play with the different API endpoints. You can find it in the admin console, under the REST API section.

![BigConnect Eplorer REST API](../../.gitbook/assets/image%20%28695%29.png)

## Web Framework

BigConnect Explorer is designed to be a single page application, but requests must be made back to the server to retrieve information, process data, and manage many other back-end services. Since BigConnect Explorer was designed to be extensible with many different types of plugins, web plugins fit naturally into BigConnect Explorer's architecture.

The Explorer has its own minimalistic, lightning-fast web framework. It provides a thin layer of fundamental web application features without adding much overhead.

Web plugins are deployed alongside Explorer inside of the web server and are used to both add custom functionality and override existing components.

The framework provides routing and parameter binding. Routing refers to determining how an application responds to a client request to a particular endpoint, which is a URI \(or path\) and a specific HTTP request method \(GET, POST, and so on\). Each route can have one or more handler functions, which are executed when the route is matched.

Route definition takes the following structure: `app.METHOD (path, handlers)` where:

* `app` in an instance of [com.mware.web.framework.App](https://github.com/bigconnect/bigconnect-explorer/blob/master/web-base/src/main/java/com/mware/web/framework/App.java)
* METHOD is an [HTTP request method](https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol#Request_methods), in lowercase.
* `path` is a path on the server
* `handlers` are a list of classes to be called when the route is matched. Each class must implement the [com.mware.web.framework.Handler](https://github.com/bigconnect/bigconnect-explorer/blob/master/web-base/src/main/java/com/mware/web/framework/Handler.java) or its sub-interfaces, like [com.mware.web.framework.ParameterizedHandler](https://github.com/bigconnect/bigconnect-explorer/blob/master/web-base/src/main/java/com/mware/web/framework/ParameterizedHandler.java)

Route handlers need to satisfy one requirement: they need to have a method annotated with the [Handle](https://github.com/bigconnect/bigconnect-explorer/blob/master/web-base/src/main/java/com/mware/web/framework/annotations/Handle.java) annotation. This method will be called by the framework once the route matches.

Handler methods can also have parameters that will be bound automatically to request parameters, based on their type or name. There are also two annotations available to bind request parameters

* [Optional](https://github.com/bigconnect/bigconnect-explorer/blob/master/web-base/src/main/java/com/mware/web/framework/annotations/Optional.java) - the parameter is optional
* [Required](https://github.com/bigconnect/bigconnect-explorer/blob/master/web-base/src/main/java/com/mware/web/framework/annotations/Required.java) - the parameter is required and the framework will throw an exception if it's not set in the incoming request

## Web Plugins

Creating a web app plugin can range from creating some JavaScript that can execute inside of Explorer all the way to overriding the current functionality of an endpoint inside Explorer. This means that web app plugins will always have some sort of mix of front-end and back-end components that provide functionality.

To start working with a web plugin, you must implement the interface [com.mware.web.WebAppPlugin](https://github.com/bigconnect/bigconnect-explorer/blob/master/web-base/src/main/java/com/mware/web/WebAppPlugin.java) and register all of your helper files in there. 

To ensure that your web plugin is deployed, it needs to be loaded onto the classpath by adding it's fully qualified class name into the `META-INF/services/com.mware.web.WebAppPlugin` file in a jar that is on the classpath.

Web Plugins can be developed using plain JavaScript or React, depending on your preference:

* Creating a standard plugin
* Creating a React plugin

BigConnect Explorer provides a rich set of UI extension points to develop or alter existing functionality, without altering the main code. The complete list is available on the Explorer Extension Points page.  



  








