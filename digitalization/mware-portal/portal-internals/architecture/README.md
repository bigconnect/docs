# Architecture

The Liferay DXP/Portal architecture has three parts:

1. **Core:** Bootstraps DXP and its [frameworks](https://learn.liferay.com/web/guest/w/dxp/building-applications/core-frameworks). The Core provides a runtime environment for managing services, UI components, and customizations.
2. **Services:** Expose DXP functionality and custom functionality via Java APIs and web APIs.
3. **UI:** The optional web application UI for adding portals, sites, pages, widgets, and content.

You can use the UI and services together or focus solely on using services via [REST APIs](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/consuming-apis/consuming-rest-services) or [GraphQL APIs](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/consuming-apis/consuming-graphql-apis).

![Sites contain content and widgets. DXP can also be used headless---without the UI.](https://resources.learn.liferay.com/images/dxp/latest/en/liferay-internals/architecture/images/01.png)

The architecture satisfies these requirements:

* Supports using common development technologies
* Leverages development standards
* Facilitates swapping components
* Starts fast and performs well
* Its runtime is easy to configure and inspect

The Core supports UI and service deployments and orchestrates wiring them together.

### [Core](https://learn.liferay.com/web/guest/w/dxp/liferay-internals/architecture#core) <a href="#core" id="core"></a>

DXP is a web application that runs on your application server. The Core bootstraps the application and its [frameworks](https://learn.liferay.com/web/guest/w/dxp/building-applications/core-frameworks).

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

* [Services](https://learn.liferay.com/web/guest/w/dxp/liferay-internals/fundamentals/apis-as-osgi-services)
* [Service customizations](https://learn.liferay.com/web/guest/w/dxp/liferay-internals/extending-liferay/overriding-osgi-services)
* [Language Keys (localized messages)](https://learn.liferay.com/web/guest/w/dxp/building-applications/developing-a-java-web-application/using-mvc/sharing-localized-messages)
* [JavaScript applications (templates, routers, and resources)](https://learn.liferay.com/web/guest/w/dxp/building-applications/developing-a-javascript-application/using-react)
* [JSP customizations](https://learn.liferay.com/web/guest/w/dxp/liferay-internals/extending-liferay/customizing-jsps)
* [Portlets (templates, controllers, and resources)](https://learn.liferay.com/web/guest/w/dxp/building-applications/developing-a-java-web-application/reference/portlets)
* [Portlet filters](https://learn.liferay.com/web/guest/w/dxp/liferay-internals/extending-liferay/auditing-portlet-activity-with-a-portlet-filter)
* [Themes](https://learn.liferay.com/web/guest/w/dxp/site-building/site-appearance/themes/introduction-to-themes)

The following figure shows these component types in the runtime environment.

![The Core provides a runtime environment for components, such as the ones here. New component implementations can extend or replace existing implementations dynamically.](https://resources.learn.liferay.com/images/dxp/latest/en/liferay-internals/architecture/images/02.png)

The runtime environment supports adding, replacing, and customizing components on-the-fly. This makes the following scenarios possible:

**Replacement:** If the `ServiceC Impl 2` component has a higher ranking than existing component `ServiceC Impl 1`, `ServiceC Impl 2` is used.

**Customization:** The `PortletA Filter` intercepts and modifies requests to and responses from `PortletA`, affecting the content `PortletA` displays.

Component WAR and [module JAR projects](https://learn.liferay.com/web/guest/w/dxp/liferay-internals/fundamentals/module-projects) install as [OSGi bundles](https://www.osgi.org/) (modules). Liferay’s OSGi framework defines the module life cycle, enforces dependencies, defines the class loading structure, and provides an API and CLI ([Felix Gogo Shell](https://learn.liferay.com/web/guest/w/dxp/liferay-internals/fundamentals/using-an-osgi-service)) for managing modules and components. The Core is configured via [Portal Properties files](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/reference/portal-properties) and [System Settings](https://learn.liferay.com/web/guest/w/dxp/system-administration/configuring-liferay/system-settings).

The service components provide business functionality.

### [Services](https://learn.liferay.com/web/guest/w/dxp/liferay-internals/architecture#services) <a href="#services" id="services"></a>

Business logic is implemented in services deployed to the component runtime environment. Built-in Core services and framework services operate on models such as [Users](https://learn.liferay.com/web/guest/w/dxp/users-and-permissions/users/understanding-users), [Roles](https://learn.liferay.com/web/guest/w/dxp/users-and-permissions/roles-and-permissions/understanding-roles-and-permissions), [Web Content](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/web-content/web-content-articles/adding-a-basic-web-content-article), [Documents and Media](https://learn.liferay.com/web/guest/w/dxp/content-authoring-and-management/documents-and-media), and more. You can introduce new models and functionality through custom services. Service components can access each other via dependency injection.

Front-end applications invoke the services to do work. You can deploy Java-based applications that call services directly using the Java APIs. Any web-based (Java and non-Java) application, whether deployed on DXP or not, can use the web APIs, including [REST APIs](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/consuming-apis/consuming-rest-services) that conform to the [OpenAPI](https://swagger.io/docs/specification/about/) standard, [GraphQL APIs](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/consuming-apis/consuming-graphql-apis), and plain web/REST services. The following figure shows applications and external clients invoking Liferay services.

![Local and remote applications can invoke services via REST web APIs. Java-based portlets can also invoke services via Java APIs.](https://resources.learn.liferay.com/images/dxp/latest/en/liferay-internals/architecture/images/03.png)

Liferay services are built using [Service Builder](https://learn.liferay.com/web/guest/w/dxp/building-applications/data-frameworks/service-builder) and made REST-ful using [REST Builder](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/apis-with-rest-builder). The services are easy to [override and extend](https://learn.liferay.com/web/guest/w/dxp/liferay-internals/extending-liferay/overriding-osgi-services) too.

The web-based UI makes content and service functionality available in browsers.

### [UI](https://learn.liferay.com/web/guest/w/dxp/liferay-internals/architecture#ui) <a href="#ui" id="ui"></a>

The UI helps people do work, [collaborate](https://learn.liferay.com/web/guest/w/dxp/collaboration-and-social/collaboration-and-social-overview), and enjoy content. Here’s what the UI consists of:

* [DXP application](https://learn.liferay.com/web/guest/w/dxp/site-building/getting-started-with-site-building): The web application for managing Portals, Sites, Users, Pages, Widgets, and more.
* [Applications](https://learn.liferay.com/web/guest/w/dxp/building-applications/developing-a-java-web-application): Widgets that provide a user interface for services already deployed.
* [Themes](https://learn.liferay.com/web/guest/w/dxp/site-building/site-appearance/themes/introduction-to-themes): Plugins for styling Sites with a unique look and feel.

The [UI architecture](https://learn.liferay.com/web/guest/w/dxp/liferay-internals/architecture/ui-architecture) articles dig deeper into developing and customizing UI components.

As you can see, the architecture supports developing services, UI components, and customizations. The architecture section covers Core, service, and UI topics. Next, dive into the Core to learn class loading, modularity, and more. But you can jump ahead to any service or [UI architecture](https://learn.liferay.com/web/guest/w/dxp/liferay-internals/architecture/ui-architecture) topics, if you like. Enjoy exploring the architecture!
