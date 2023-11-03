# Standard Classloading

Class loaders in Payara Server follow a delegation hierarchy by default. This page documents how classes are usually loaded and from which locations.

### The Class Loader Hierarchy

With the default classloader configuration, Payara Server will attempt to search for classes in the following locations in this order:

1. Classes provided by the JDK
2. Extension libraries in a Payara Server domain (installed as a library of the `ext` type )
3. Classes provided by Payara Server
4. Common libraries in a Payara Server domain (installed as a library of the `common` type)
5. Application-specific libraries in Payara Server (installed as a library of the `app` type)
6. Classes and libraries provided by the deployed application archive (e.g. `WAR` or `EAR`)

Classloaders that are lower in the hierarchy will first delegate searching for a class to a classloader higher in the hierarchy.

Here, you can see that:

* Classes provided by Payara Server have higher priority than classes provided by an application
* Classes provided by Payara Server have higher priority than installed libraries of type `common` and `app`
* Classes in installed libraries have higher priority than classes provided by an application
* To install a library with a higher priority than classes provided by Payara Server, it should be installed with type `ext`

The delegation hierarchy can be tweaked using the [Enhanced Classloading](https://docs.payara.fish/community/docs/Technical%20Documentation/Payara%20Server%20Documentation/Server%20Configuration%20And%20Management/Classloading/Enhanced%20Classloading.html) features. For example, it’s possible to give the classes provided by an application the highest priority (by disabling classloading delegation), or completely hide (isolate) classes provided by Payara Server from a deployed application.

### Install Libraries in a Payara Server Domain

If a deployed application requires additional classes not provided by the application, you can install a library with those classes into the Payara Server domain. This is useful if you’d like to build an application for flexible deployments and configure it with classes dropped on the classpath, or if you’d like multiple deployed applications to share the same library.

If an internal Payara Server resource, such as a JDBC connection pool, requires an additional library, you can install it into the Payara Server domain too.

To install a library into a Payara Server domain, use the asadmin [add-library](https://docs.payara.fish/community/docs/Technical%20Documentation/Payara%20Server%20Documentation/Server%20Configuration%20And%20Management/Asadmin%20Commands/Server%20Management%20Asadmin%20Commands.html#add-library-command) command. This command accepts the `--type` argument, which accepts the following options:

`common`

The library is installed as a common library into the `lib` directory in the Payara Server domain

`ext`

The library is installed as an extension library into the `lib/ext` directory in the Payara Server domain

`app`

the library is installed as an application library into the `lib/applibs` directory in the Payara Server domain

### Common Libraries

Common libraries are available to all applications or modules deployed on servers with the same configuration. There are several levels of common libraries:

* Shared by all applications deployed on the same Payara Server installation - located in the `glassfish/lib` directory in the Payara Server installation
* Shared by all applications deployed in the same Payara Server domain - located in the `lib` directory in that Payara Server domain directory
* Shared by all applications deployed on instances that share the same configuration - located in the `config/<config-name>/lib` directory in the particular Payara Server domain directory

### Extension Libraries

Application developers can use libraries installed as extension libraries to extend the functionality of the core Payara Server platform. For example, an Oracle Database JDBC driver should be installed as an extension library if it’s going to be used via JPA to use Oracle-specific JPA features. Or a Java agent library should be installed as an extension library if the Java agent instruments classes in Payara Server to use classes from the agent library itself.

#### Extension Libraries Support On JDK 8

When Payara Server runs on JDK 8, extension libraries are added to the JVM using the standard [Java extension mechanism](https://docs.oracle.com/javase/tutorial/ext/index.html), by adding the `lib/ext` directory in the current domain as an extension directory using the `java.ext.dirs` JVM option.

#### Extension Libraries Support On JDK 11+

When Payara Server runs on JDK 11 and newer releases, extension libraries are added to the system classpath at the beginning of the classpath, so that they have higher priority than all other classes and libraries on the classpath. This is because the Java extension mechanism isn’t supported in Java 11 and newer and the official recommend way to replace it is to add the libraries to the classpath directly.

This behavior is triggered automatically if the `java.ext.dirs` property isn’t defined as a JVM option, regardless of the JDK version used. Since defining the `java.ext.dirs` on Java 11+ leads to an error, this JVM option cannot be used with Java 11+ and extension libraries are automatically added as classpath elements.
