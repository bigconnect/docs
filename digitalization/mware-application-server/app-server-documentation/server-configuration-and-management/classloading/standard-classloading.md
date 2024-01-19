# Standard Classloading

Class loaders in MWARE AS follow a delegation hierarchy by default. This page documents how classes are usually loaded and from which locations.

### The Class Loader Hierarchy

With the default classloader configuration, MWARE AS will attempt to search for classes in the following locations in this order:

1. Classes provided by the JDK
2. Extension libraries in a MWARE AS domain (installed as a library of the `ext` type )
3. Classes provided by MWARE AS
4. Common libraries in a MWARE AS domain (installed as a library of the `common` type)
5. Application-specific libraries in MWARE AS (installed as a library of the `app` type)
6. Classes and libraries provided by the deployed application archive (e.g. `WAR` or `EAR`)

Classloaders that are lower in the hierarchy will first delegate searching for a class to a classloader higher in the hierarchy.

Here, you can see that:

* Classes provided by MWARE AS have higher priority than classes provided by an application
* Classes provided by MWARE AS have higher priority than installed libraries of type `common` and `app`
* Classes in installed libraries have higher priority than classes provided by an application
* To install a library with a higher priority than classes provided by MWARE AS, it should be installed with type `ext`

The delegation hierarchy can be tweaked using the Enhanced Classloading features. For example, it’s possible to give the classes provided by an application the highest priority (by disabling classloading delegation), or completely hide (isolate) classes provided by MWARE AS from a deployed application.

### Install Libraries in a MWARE AS Domain

If a deployed application requires additional classes not provided by the application, you can install a library with those classes into the MWARE AS domain. This is useful if you’d like to build an application for flexible deployments and configure it with classes dropped on the classpath, or if you’d like multiple deployed applications to share the same library.

If an internal MWARE AS resource, such as a JDBC connection pool, requires an additional library, you can install it into the MWARE AS domain too.

To install a library into a MWARE AS domain, use the asadmin add-library command. This command accepts the `--type` argument, which accepts the following options:

`common`

The library is installed as a common library into the `lib` directory in the MWARE AS domain

`ext`

The library is installed as an extension library into the `lib/ext` directory in the MWARE AS domain

`app`

the library is installed as an application library into the `lib/applibs` directory in the MWARE AS domain

### Common Libraries

Common libraries are available to all applications or modules deployed on servers with the same configuration. There are several levels of common libraries:

* Shared by all applications deployed on the same MWARE AS installation - located in the `glassfish/lib` directory in the MWARE AS installation
* Shared by all applications deployed in the same MWARE AS domain - located in the `lib` directory in that MWARE AS domain directory
* Shared by all applications deployed on instances that share the same configuration - located in the `config/<config-name>/lib` directory in the particular MWARE AS domain directory

### Extension Libraries

Application developers can use libraries installed as extension libraries to extend the functionality of the core MWARE AS platform. For example, an Oracle Database JDBC driver should be installed as an extension library if it’s going to be used via JPA to use Oracle-specific JPA features. Or a Java agent library should be installed as an extension library if the Java agent instruments classes in MWARE AS to use classes from the agent library itself.

#### Extension Libraries Support On JDK 8

When MWARE AS runs on JDK 8, extension libraries are added to the JVM using the standard [Java extension mechanism](https://docs.oracle.com/javase/tutorial/ext/index.html), by adding the `lib/ext` directory in the current domain as an extension directory using the `java.ext.dirs` JVM option.

#### Extension Libraries Support On JDK 11+

When MWARE AS runs on JDK 11 and newer releases, extension libraries are added to the system classpath at the beginning of the classpath, so that they have higher priority than all other classes and libraries on the classpath. This is because the Java extension mechanism isn’t supported in Java 11 and newer and the official recommend way to replace it is to add the libraries to the classpath directly.

This behavior is triggered automatically if the `java.ext.dirs` property isn’t defined as a JVM option, regardless of the JDK version used. Since defining the `java.ext.dirs` on Java 11+ leads to an error, this JVM option cannot be used with Java 11+ and extension libraries are automatically added as classpath elements.
