# Architecture

MWARE Portal is an open system, based on SOA principles, that provides a seamless architecture designed for extending and exposing portal features to developers and application integrators.

The core tehnologies used are: HTML5, CSS3, AJAX, SOA, iCalendar, Microformat, JSR-362 (Portlet 3.0), JSR-371 (MVC1.0), JSR-344 (JSF 2.2), Spring 3.0, JSP, Servlets, SQL, CMIS 1.0/1.1, Hibernate, OSGi Core 6.0 and SAML 1.0, 1.1, 2.0.

The portal architecture has three parts:

1. **Core:** Bootstraps the portal and its frameworks. The Core provides a runtime environment for managing services, UI components, and customizations.
2. **Services:** Expose portal functionality and custom functionality via Java APIs and web APIs.
3. **UI:** The optional web application UI for adding portals, sites, pages, widgets, and content.

You can use the UI and services together or focus solely on using services via REST APIs or GraphQL APIs.

![Sites contain content and widgets. The portal can also be used headless---without the UI.](https://resources.learn.liferay.com/images/dxp/latest/en/liferay-internals/architecture/images/01.png)

The architecture satisfies these requirements:

* Supports using common development technologies
* Leverages development standards
* Facilitates swapping components
* Starts fast and performs well
* Its runtime is easy to configure and inspect

The Core supports UI and service deployments and orchestrates wiring them together.

### Core <a href="#core" id="core"></a>

The portal is a web application that runs on your application server. The Core bootstraps the application and its frameworks.

There are frameworks for these things and more:

* Adaptive Media
* Application Configuration
* Application Security
* Assets
* Cache
* Data Scopes
* Dependency Injection
* Expando/Custom Attributes
* File Management
* Localization
* Logging
* Message Bus
* Scheduler
* Search
* Segmentation and Personalization
* Service Builder
* Testing
* Upgrade Processes
* Virus Protection
* Workflow

The Core provides the component runtime environment for the frameworks, services, and UI. Here are some component examples:

* Services
* Service customizations
* Language Keys (localized messages)
* JavaScript applications (templates, routers, and resources)
* JSP customizations
* Portlets (templates, controllers, and resources)
* Portlet filters
* Themes

The following figure shows these component types in the runtime environment.

![The Core provides a runtime environment for components, such as the ones here. New component implementations can extend or replace existing implementations dynamically.](https://resources.learn.liferay.com/images/dxp/latest/en/liferay-internals/architecture/images/02.png)

The runtime environment supports adding, replacing, and customizing components on-the-fly. This makes the following scenarios possible:

**Replacement:** If the `ServiceC Impl 2` component has a higher ranking than existing component `ServiceC Impl 1`, `ServiceC Impl 2` is used.

**Customization:** The `PortletA Filter` intercepts and modifies requests to and responses from `PortletA`, affecting the content `PortletA` displays.

Component WAR and module JAR projects install as OSGi bundles (modules). The portal’s OSGi framework defines the module life cycle, enforces dependencies, defines the class loading structure, and provides an API and CLI (Felix Gogo Shell) for managing modules and components. The Core is configured via Portal Properties files and System Settings.

The service components provide business functionality.

### Services <a href="#services" id="services"></a>

Business logic is implemented in services deployed to the component runtime environment. Built-in Core services and framework services operate on models such as Users, Roles, Web Content, Documents and Media, and more. You can introduce new models and functionality through custom services. Service components can access each other via dependency injection.

Front-end applications invoke the services to do work. You can deploy Java-based applications that call services directly using the Java APIs. Any web-based (Java and non-Java) application can use the web APIs, including REST APIs that conform to the [OpenAPI](https://swagger.io/docs/specification/about/) standard, GraphQL APIs, and plain web/REST services. The following figure shows applications and external clients invoking portal services.

![Local and remote applications can invoke services via REST web APIs. Java-based portlets can also invoke services via Java APIs.](https://resources.learn.liferay.com/images/dxp/latest/en/liferay-internals/architecture/images/03.png)

Portal services are built using Service Builder and made REST-ful using REST Builder. The services are easy to override and extend too.

The web-based UI makes content and service functionality available in browsers.

### UI <a href="#ui" id="ui"></a>

The UI helps people do work, collaborate, and enjoy content. Here’s what the UI consists of:

* Portal application: The web application for managing Portals, Sites, Users, Pages, Widgets, and more.
* Applications: Widgets that provide a user interface for services already deployed.
* Themes: Plugins for styling Sites with a unique look and feel.

The UI architecture articles dig deeper into developing and customizing UI components.

As you can see, the architecture supports developing services, UI components, and customizations. The architecture section covers Core, service, and UI topics. Next, dive into the Core to learn class loading, modularity, and more. But you can jump ahead to any service or UI architecture topics, if you like.
