# Module Projects

Applications and customizations are [OSGi modules](https://www.osgi.org/resources/what-is-osgi/): `.jar` files containing Java code and some extra configuration for publishing and consuming APIs.

A module project comprises three things:

1. **Code:** Java classes and resources, such as images, templates, and additional descriptors. Java packages are private by default but can be exported for other modules to use.
2. **Build Scripts:** [Gradle](https://gradle.org/) files for building and deploying the module.
3. **Metadata:** A [Bnd](https://bnd.bndtools.org/) file defines the module artifact and specifies packages and capabilities the module provides and requires.

Here’s the module project structure:

```log
[project root]
 └── [module 1]
 │    ├── bnd.bnd // Defines the module artifact, provided/required capabilities, and more
 │    ├── build.gradle // Declares dependencies
 │    └── src
 │        └── main
 │            ├── java
 │            │   └── [Java packages]
 │            └── resources
 │                └── [Images, templates, descriptors, etc.]
 │
 └── [module 2]
 │
 └── [module n]
 │
 ├── gradle
 │   └── [Gradle wrapper files]
 ├── gradlew // Invokes the Gradle wrapper to execute tasks
 ├── gradlew.bat
 ├── gradle.properties // Specifies the product version
 └── settings.gradle // Applies Gradle plugins
```

The portal commonly uses three kinds of modules:

1. **API** modules define interfaces.
2. **Implementation** modules provide concrete classes that implement interfaces.
3. **Client** modules consume the APIs.

You’ll learn how to create each one by developing a simple command in Gogo Shell to greet users when they enter their names.

![Gogo shell command that greets users.](https://resources.learn.liferay.com/images/dxp/latest/en/liferay-internals/fundamentals/module-projects/images/01.png)

Here you’ll create the API, learn the parts of a module project, deploy the module, and inspect the module at runtime. You’ll create the implementation module and client module in the next two tutorials.

Start with deploying the example API module project.

### Deploy a Simple Module <a href="#deploy-a-simple-module" id="deploy-a-simple-module"></a>

The example module defines an API for generating a greeting.

Start a new portal instance by running

```bash
docker run -it -m 8g -p 8080:8080 liferay/portal:7.4.3.112-ga112
```

Sign in to the portal at [http://localhost:8080](http://localhost:8080/). Use the email address _test@liferay.com_ and the password _test_. When prompted, change the password to _learn_.

Then, follow these steps:

1.  Download and unzip the example.

    ```bash
    curl https://resources.learn.liferay.com/dxp/latest/en/liferay-internals/fundamentals/liferay-k8s2.zip -O
    ```

    ```bash
    unzip liferay-k8s2.zip
    ```
2.  Build the module JAR.

    ```bash
    cd liferay-k8s2
    ```

    ```bash
    ./gradlew jar
    ```

    The JAR file is generated to the module’s `build/libs` folder.

    ```log
    k8s2-api/build/libs/com.acme.k8s2.api-1.0.0.jar
    ```
3.  Deploy the module JAR.

    ```bash
    ./gradlew deploy -Ddeploy.docker.container.id=$(docker ps -lq)
    ```

    Log messages show the portal processing the JAR and starting the module.

    ```log
    Processing com.acme.k8s2.api-1.0.0.jar
    STARTED com.acme.k8s2.api_1.0.0 [1152]
    ```

    The `STARTED` message includes module’s ID: `1152`
4. Open the Gogo Shell.
5.  In the Gogo Shell command field, use `lb` to show the module’s information, including its ID. The most recently added module appears last. If you know a keyword in the module name, you can `grep` for it.

    ```bash
    lb | grep -i "k8s2"
    ```

    Output:

    ```log
    1152|Active     |   15|Acme K8S2 API (1.0.0)|1.0.0
    ```

    This module’s ID is `1152`.
6.  Use the `b` command and the module ID to show more information about the module.

    ```bash
    b 1152
    ```

    Output:

    ```log
    com.acme.k8s2.api_1.0.0 [1152]
    Id=1152, Status=ACTIVE      Data Root=[Home]/osgi/state/org.eclipse.osgi/1152/data
      "No registered services."
      No services in use.
      Exported packages
        com.acme.k8s2; version="1.0.0"[exported]
      No imported packages
      No fragment bundles
      No required bundles
    ```

The module is active and exports a package called `com.acme.k8s2`.

Now that you have installed and activated the module, you can learn how it works.

### How to Configure a Module <a href="#how-to-configure-a-module" id="how-to-configure-a-module"></a>

* Set Up the Build Infrastructure
* Write Code
* Specify Dependencies
* Specify Metadata

#### Set Up the Build Infrastructure <a href="#set-up-the-build-infrastructure" id="set-up-the-build-infrastructure"></a>

The portal modules are developed in a Gradle build infrastructure. The following Gradle files are in the project’s root folder.

| File                | Description                                                    |
| ------------------- | -------------------------------------------------------------- |
| `gradle/`           | Contains a Gradle wrapper                                      |
| `gradlew[.bat]`     | Invokes the Gradle wrapper to execute tasks                    |
| `gradle.properties` | Specifies the product version                                  |
| `settings.gradle`   | Applies Gradle plugins, including the portal Workspace plugin. |

You can add more modules in new subfolders, like the example project’s `k8s2-api` folder, or create them in a new Workspace.

Here’s the `k8s2-api` module structure in the context of the project root.

```log
[project root]
 └── k8s2-api
 │   ├── bnd.bnd
 │   ├── build.gradle
 │   └── src
 │       └── main
 │           └── java
 │               └── com/acme/k8s2
 │                   └── Greeter.java
 │
 └── [Gradle files]
```

The `k8s2-api` module folder contains a `bnd.bnd` metadata file, a `build.gradle` script, and Java code.

#### Write Code <a href="#write-code" id="write-code"></a>

The example module has only one Java class: an interface called `Greeter`.

```java
@ProviderType
public interface Greeter {

	public void greet(String name);

}
```

The [`@ProviderType`](https://docs.osgi.org/javadoc/osgi.annotation/7.0.0/org/osgi/annotation/versioning/ProviderType.html) annotation tells the service registry that anything implementing the interface provides it (i.e., a `Greeter`). The interface’s one method called `greet` asks for a `String` and doesn’t return anything.

Add your own Java code and resources in your module’s `src/main/java` folder and `src/main/resources` folder, respectively.

#### Specify Dependencies <a href="#specify-dependencies" id="specify-dependencies"></a>

The `build.gradle` file specifies the module’s dependencies.

```groovy
dependencies {
	compileOnly group: "com.liferay.portal", name: "release.portal.api"
}
```

It depends on one artifact: the release API JAR. It is a large JAR packed with the portal, Bnd, and OSGi artifacts associated with the product release.

In the `[project root]/gradle.properties` file, the `liferay.workspace.product` property specifies the product release:

```properties
liferay.workspace.product=portal-7.4-ga112
```

Lastly, there’s no dependency version. That’s because Workspace applies the product API version associated with the release.

NOTE

Please see Configuring Dependencies for more information.

#### Specify Metadata <a href="#specify-metadata" id="specify-metadata"></a>

The module JAR’s `META-INF/MANIFEST.MF` file describes the module. The manifest contains properties called manifest headers that specify packages the module exports/imports and capabilities the module provides/requires. Since the build infrastructure provides Bnd, you need only specify a few initial headers in your module’s `bnd.bnd` file. Bnd generates most other values based on its inspection of your module.

**Initial Metadata**

The `bnd.bnd` file describes and configures the module.

```java
Bundle-Name: Acme K8S2 API
Bundle-SymbolicName: com.acme.k8s2.api
Bundle-Version: 1.0.0
Export-Package: com.acme.k8s2
```

The module’s name is _Acme K8S2 API_. Its symbolic name—a name that ensures uniqueness—is `com.acme.k8s2.api`. Its semantic version is declared next. Lastly, the module _exports_ the Java package `com.acme.k8s2`, making the package available to other modules. You confirmed the package export above when you executed the `b [bundle ID]` Gogo Shell command.

**Generated Metadata**

At build time, Bnd propagates metadata from the `bnd.bnd` file to the JAR file’s `META-INF/MANIFEST.MF` and adds metadata based on its inspection.

Here’s a `META-INF/MANIFEST.MF` file generated for the example module:

```properties
Manifest-Version: 1.0
Bnd-LastModified: 1598968383025
Bundle-ManifestVersion: 2
Bundle-Name: Acme K8S2 API
Bundle-SymbolicName: com.acme.k8s2.api
Bundle-Version: 1.0.0
Created-By: 1.8.0_252 (Oracle Corporation)
Export-Package: com.acme.k8s2;version="1.0.0"
Javac-Debug: on
Javac-Deprecation: off
Javac-Encoding: UTF-8
Require-Capability: osgi.ee;filter:="(&(osgi.ee=JavaSE)(version=1.8))"
Tool: Bnd-4.3.0.201909301554
```

Bnd propagated all the headers from the `bnd.bnd` file and added more headers and details. For example, the exported `com.acme.k8s2` package has the default package version `1.0.0`.

### Conclusion <a href="#conclusion" id="conclusion"></a>

That’s it! As you can see, module projects are the same as other Java projects, with some added configuration.

Now you know what module projects look like, how to build and deploy them, and how to inspect modules at runtime.

Modules leverage each other’s capabilities via APIs like the `Greeter` API. The portal uses OSGi Services to define, implement, and consume APIS. Next, APIs as OSGi Services demonstrates _implementing_ the `Greeter` API using OSGi services.
