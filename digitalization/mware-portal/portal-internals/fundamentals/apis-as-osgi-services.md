# APIs as OSGi Services

After you’ve learned what a module is and how to deploy one, you can use modules to define APIs and implement them. APIs are [OSGi services](https://enroute.osgi.org/), defined by Java interfaces and implemented by concrete Java classes.

The portal exposes APIs, implementations, and clients as components. [OSGi Declarative Services](https://enroute.osgi.org/FAQ/300-declarative-services.html) (DS) annotations define components and their relationships.

* [`@ProviderType`](https://docs.osgi.org/javadoc/osgi.annotation/7.0.0/org/osgi/annotation/versioning/ProviderType.html) defines an interface that components can provide (implement) or consume.
* [`@Component`](https://docs.osgi.org/javadoc/osgi.cmpn/7.0.0/org/osgi/service/component/annotations/Component.html) declares the class a component, providing a particular capability.
* [`@Reference`](https://docs.osgi.org/javadoc/osgi.cmpn/7.0.0/org/osgi/service/component/annotations/Reference.html) wires another component to a class member (typically a field).

You can separate API and implementation concerns into different modules.

* **API** modules _define_ capabilities using Java interfaces. The modules export the interface packages.
* **Implementation** modules _provide_ capabilities using concrete Java classes.

Here you’ll deploy API and implementation modules that create a simple greeter OSGi service. You’ll also examine the implementation module and its JAR to learn how the implementation provides the greeter service capability. In the next tutorial, you’ll create the client—the part that you can invoke in the UI.

### Deploy a Simple API and Implementation <a href="#deploy-a-simple-api-and-implementation" id="deploy-a-simple-api-and-implementation"></a>

Start a new portal instance by running

```bash
docker run -it -m 8g -p 8080:8080 liferay/portal:7.4.3.112-ga112
```

Sign in to the portal at [http://localhost:8080](http://localhost:8080/). Use the email address _test@liferay.com_ and the password _test_. When prompted, change the password to _learn_.

Then, follow these steps to start the example modules:

1.  Download and unzip `liferay-p9g2.zip`.

    ```bash
    curl https://resources.learn.liferay.com/dxp/latest/en/liferay-internals/fundamentals/liferay-p9g2.zip -O
    ```

    ```bash
    unzip liferay-p9g2.zip
    ```
2.  From the project root folder, deploy the modules.

    ```bash
    cd liferay-p9g2
    ```

    ```bash
    ./gradlew deploy -Ddeploy.docker.container.id=$(docker ps -lq)
    ```
3.  Confirm module startup in the Docker container console.

    ```log
    STARTED com.acme.p9g2.api_1.0.0
    STARTED com.acme.p9g2.impl_1.0.0
    ```
4. Go to `http://localhost:8080` and sign in.
5. Go to the Gogo Shell.
6.  Get the module IDs using the `lb` Gogo Shell command.

    ```bash
    g! lb | grep -i "Acme P9G2"
    ```

    Output:

    ```log
    1150|Active     |   15|Acme P9G2 API (1.0.0)|1.0.0
    1151|Active     |   15|Acme P9G2 Implementation (1.0.0)|1.0.0
    ```
7.  List the implementation module service capabilities by executing the following command, replacing the number with your module’s ID.

    ```bash
    g! inspect capability service 1195
    ```

    Output:

    ```log
    com.acme.p9g2.impl_1.0.0 [1151] provides:
    -----------------------------------------
    service; com.acme.p9g2.Greeter with properties:
       service.id = 22933
       service.bundleid = 1151
       service.scope = bundle
       component.name = com.acme.p9g2.internal.P9G2Greeter
       component.id = 8462
    ```

The Acme P9G2 Implementation module provides one service: `com.acme.p9g2.Greeter`. The `component.name` property indicates that the module’s `com.acme.p9g2.internal.P9G2Greeter` component implements the service.

You verified that the `P9G2Greeter` component provides the `Greeter` service.

Next you’ll learn how the API module defines the greeter capability and the implementation module provides the greeter capability as an OSGi service. Start with creating the API.

### Create the API <a href="#create-the-api" id="create-the-api"></a>

An API is created in just two steps:

* Define the Capability
* Export the Interface Package

#### Define the Capability <a href="#define-the-capability" id="define-the-capability"></a>

The example API module `Greeter` class is a Java interface.

```java
@ProviderType
public interface Greeter {
```

The [`@ProviderType`](https://docs.osgi.org/javadoc/osgi.annotation/7.0.0/org/osgi/annotation/versioning/ProviderType.html) annotation registers `Greeter` as a type that a component can implement or consume.

The `greet` method takes a name `String` as input.

```java
public void greet(String name);
```

The `Greeter` capability is defined.

#### Export the Interface Package <a href="#export-the-interface-package" id="export-the-interface-package"></a>

The API module `bnd.bnd` file describes the module and exports the `com.acme.p9g2` interface package.

```java
Bundle-Name: Acme P9G2 API
Bundle-SymbolicName: com.acme.p9g2.api
Bundle-Version: 1.0.0
Export-Package: com.acme.p9g2
```

The package export shares the `Greeter` interface with other modules.

The `Greeter` service type is available to implement and use.

### Create the Implementation <a href="#create-the-implementation" id="create-the-implementation"></a>

The example implementation module contains a concrete Java class that provides the `Greeter` capability. Here are the implementation steps.

* Add the Component Annotion Class
* Implement the Interface
* Add a Dependency on the API
* Examine the Module JAR

#### Add the Component Annotation <a href="#add-the-component-annotation" id="add-the-component-annotation"></a>

The `P9G2Greeter` class implements the `Greeter` interface:

```java
@Component(service = Greeter.class)
public class P9G2Greeter implements Greeter {
```

The [`@Component`](https://docs.osgi.org/javadoc/osgi.cmpn/7.0.0/org/osgi/service/component/annotations/Component.html) annotation and its `service = Greeter.class` attribute make the `P9G2Greeter` class a `Greeter` service provider.

#### Implement the Interface <a href="#implement-the-interface" id="implement-the-interface"></a>

The `Greeter` interface defines a method `greet(String)` with a `void` return value.

```java
@Override
public void greet(String name) {
	System.out.println("Hello " + name + "!");
}
```

The example `greet` method prints an enthusiastic greeting using the given name.

#### Add a Dependency on the API <a href="#add-a-dependency-on-the-api" id="add-a-dependency-on-the-api"></a>

Here is the implementation module `build.gradle` file.

```groovy
dependencies {
	compileOnly group: "com.liferay.portal", name: "release.portal.api"
	compileOnly project(":p9g2-api")
}
```

It includes a compile-time dependency on the `p9g2-api` module project because it requires the module’s `Greeter` class.

#### Examine the Module JAR <a href="#examine-the-module-jar" id="examine-the-module-jar"></a>

When you built the `p9g2-impl/build/libs/com.acme.p9g2.impl-1.0.0.jar` implementation module JAR, [Bnd](http://bnd.bndtools.org/) generated the JAR’s `/META-INF/MANIFEST.MF` file.

Here are key service-related headers that Bnd generates in the manifest:

```properties
Import-Package: com.acme.p9g2;version="[1.0,2)"
```

The `Import-Package` header imports the API module’s public package that contains the `Greeter` service definition.

```properties
Provide-Capability: osgi.service;objectClass:List<String>="com.acme.p9
 g2.Greeter";uses:="com.acme.p9g2"
```

The `Provide-Capability` header configures the `P9G2Greeter` component service.

```properties
Service-Component: OSGI-INF/com.acme.p9g2.internal.P9G2Greeter.xml
```

The `Service-Component` header lists a configuration file (`.xml`) for each of the module’s service components.

When you deployed the module, the Service Component Runtime registered the `P9G2Greeter` service component as providing the `Greeter` service.

### Conclusion <a href="#conclusion" id="conclusion"></a>

You have _defined_ a service capability called `Greeter` and _provided_ it in a service component called `P9G2Greeter`. The `Greeter` service is in place. How do clients access the service and use it? That’s demonstrated in Using an OSGi Service.

\
