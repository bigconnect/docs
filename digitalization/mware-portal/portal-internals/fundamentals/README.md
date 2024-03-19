# Fundamentals

Liferay development projects consist primarily of simple .jar files. These contain a few extra configuration files that make them OSGi modules, but they’re easily understandable by anyone who knows Java.

Part of the power of OSGi is that you have a single, consistent way to build projects that access DXP’s API, extend and override various features, or package new software for deployment. These projects, or modules, form an ecosystem where they can depend on each other for functionality and be manipulated at runtime.

These are the fundamentals of Liferay DXP’s ecosystem.

* [**Module Projects**](https://learn.liferay.com/web/guest/w/dxp/liferay-internals/fundamentals/module-projects) explains what an OSGi module is, its project structure, and how it is used by Liferay’s OSGi framework.
* [**Configuring Dependencies**](https://learn.liferay.com/web/guest/w/dxp/liferay-internals/fundamentals/configuring-dependencies) demonstrates how to identify and configure Liferay artifacts and third-party artifacts to use their Java packages in your projects.
* [**Importing**](https://learn.liferay.com/web/guest/w/dxp/liferay-internals/fundamentals/importing-packages) **and** [**Exporting Packages**](https://learn.liferay.com/web/guest/w/dxp/liferay-internals/fundamentals/exporting-packages) shows how to use functionality from other modules and publish your functionality for other modules to use. Liferay’s tooling detects package use and specifies package imports automatically.
* [**Semantic Versioning**](https://learn.liferay.com/web/guest/w/dxp/liferay-internals/fundamentals/semantic-versioning) describes how Liferay manages dependencies on code as it evolves to new versions, so that you can use the same mechanism to manage your version and avoid code breakage.
* [**Gogo Shell**](https://learn.liferay.com/web/guest/w/dxp/liferay-internals/fundamentals/using-the-gogo-shell) shows how you can examine components, debug issues, and manage deployments at runtime by issuing commands into a shell.
