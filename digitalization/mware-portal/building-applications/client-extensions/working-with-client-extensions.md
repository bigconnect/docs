# Working with Client Extensions

Using client extensions, you can extend Liferay without deploying OSGi modules. Just like traditional module development, client extensions live in a [Liferay Workspace](https://learn.liferay.com/web/guest/w/dxp/building-applications/tooling/liferay-workspace). Get started by learning

* The required tools to begin developing client extensions
* How to define a client extension in its configuration file
* How to deploy client extensions

### [Required Tools and Settings](https://learn.liferay.com/web/guest/w/dxp/building-applications/client-extensions/working-with-client-extensions#required-tools-and-settings) <a href="#required-tools-and-settings" id="required-tools-and-settings"></a>

Install the three prerequisites for developing client extensions:

1.  Java 8 or JDK 11.

    NOTE

    Check the [compatibility matrix](https://help.liferay.com/hc/en-us/articles/4411310034829-Liferay-DXP-7-4-Compatibility-Matrix) for information on supported JDKs, databases, and environments. See [JVM Configuration](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/reference/jvm-configuration) for recommended JVM settings.
2.  Liferay workspace. To download a workspace with example client extension projects, run

    ```bash
    curl -o com.liferay.sample.workspace-latest.zip https://repository.liferay.com/nexus/service/local/artifact/maven/content\?r\=liferay-public-releases\&g\=com.liferay.workspace\&a\=com.liferay.sample.workspace\&\v\=LATEST\&p\=zip
    ```

    You can copy the client extension projects (in the `client-extensions/` directory) into your own workspace or use the sample workspace directly.
3. If you are using Liferay SaaS, you must have the [`lcp` CLI tool](https://learn.liferay.com/en/w/liferay-cloud/reference/command-line-tool).

### [Client Extension Projects](https://learn.liferay.com/web/guest/w/dxp/building-applications/client-extensions/working-with-client-extensions#client-extension-projects) <a href="#client-extension-projects" id="client-extension-projects"></a>

Client extension development follows a _workspace plus project_ model. Within a [Liferay workspace](https://learn.liferay.com/web/guest/w/dxp/building-applications/tooling/liferay-workspace), you implement a client extension project under `[workspace-root]/client-extensions`. The project’s `client-extension.yaml` file defines its client extensions, and a build process results in a single set of outputs for each project. The built client extension project is a deployable `*.zip` archive called a Liferay Universal File Format Archive (LUFFA).

### [Grouping Client Extensions in Projects](https://learn.liferay.com/web/guest/w/dxp/building-applications/client-extensions/working-with-client-extensions#grouping-client-extensions-in-projects) <a href="#grouping-client-extensions-in-projects" id="grouping-client-extensions-in-projects"></a>

Client extensions grouped in a single project comprise a single, deployable unit when built. You can group client extensions where it makes sense (e.g., to improve efficiency when they should work on related tasks), but there are restrictions.

Because all client extensions in a single project are associated with a Docker container that represents a workload particular to that project, only certain kinds of client extensions are compatible for grouping. For example, [microservice client extensions](https://learn.liferay.com/web/guest/w/dxp/building-applications/client-extensions/microservice-client-extensions) can only be grouped with configuration client extensions, as microservices represent a workload that runs outside Liferay.

You can group client extensions together in these ways:

* Client extensions of the same type (e.g., multiple batch client extensions)
* Configuration client extensions with batch client extensions
* Configuration client extensions with front-end client extensions
* Configuration client extensions with microservice client extensions

Building a project with an incompatible grouping of client extensions (e.g., front-end with microservice) fails with an error.

### [Configuring Client Extensions](https://learn.liferay.com/web/guest/w/dxp/building-applications/client-extensions/working-with-client-extensions#configuring-client-extensions) <a href="#configuring-client-extensions" id="configuring-client-extensions"></a>

Client extensions are defined in `client-extension.yaml` files with these properties:

`name`: Enter the name as it appears in the Liferay UI. If it’s not configurable in the UI, the `name` value is not used.

`type`: Set the client extension’s type (e.g., `themeCSS`). The type determines how Liferay handles the client extension when it’s deployed.

`dxp.lxc.liferay.com.virtualInstanceId`: Enter the virtual instance ID to deploy to.

Each client extension project has its own folder inside the workspace’s `client-extensions/` folder. A client extension project contains a single `client-extension.yaml` file that defines one or more client extensions. For example, [the `iframe-2` project’s `client-extension.yaml`](https://github.com/liferay/liferay-portal/blob/master/workspaces/liferay-sample-workspace/client-extensions/liferay-sample-iframe-2/client-extension.yaml) defines three `iframe` client extensions: `Baseball`, `Football`, and `Hockey`.

#### [Assembling Client Extensions](https://learn.liferay.com/web/guest/w/dxp/building-applications/client-extensions/working-with-client-extensions#assembling-client-extensions) <a href="#assembling-client-extensions" id="assembling-client-extensions"></a>

When you build a client extension, files are created automatically and packaged in the resulting [LUFFA](https://learn.liferay.com/web/guest/w/dxp/building-applications/client-extensions/packaging-client-extensions). Define an `assemble` block in your `client-extension.yaml` file to configure files to include from your build or project files.

The `assembleClientExtension` Gradle task executes when you run `gradle build` or `gradle deploy` within a client extension project. During execution, the files specified in the project’s `assemble` block are placed into a `build/liferay-client-extension-build/` folder in your project. Everything in this folder is used to create the LUFFA (e.g., `dist/my-client-extension-project.zip`).

NOTE

If your client extension project contains a `package.json` file with a `build` script defined, the script automatically executes when you build your project. This part of the build happens _before_ files are copied, so you can specify the task’s output location in your `assemble` block.

The `assemble` block is a YAML array that can include multiple instructions for files to include. Each set of instructions follows this pattern:

```yaml
- from: [some folder in your project]
  include: [single file or glob match]
  into: [output location in archive]
```

The `assemble` array has these properties:

* `from`: Specify the folder from which to copy files into your client extension archive.
*   `include`: Specify a single file or glob matching a subset of files to include from the from directory. If not defined, all files are included recursively (equivalent to `**/*`).

    You can use an array of multiple `include` patterns if needed:

    ```yaml
    assemble:
        - from: build
          include:
            - "vite/js/*.js"
            - "vite/css/*.css"
          into: static
    ```
*   `into`: Specify where in the resulting LUFFA to copy the matching resources.

    Static resources for front-end client extensions must be copied into the `static/` directory. Liferay serves these as static resources in self-hosted instances, or from containers in Liferay SaaS.

    JSON resources for batch client extensions must be copied into the `batch/` directory.
*   `fromTask`: Instead of `from`, you can specify a Gradle task in the project to execute before the assembly step.

    For example, in a `microservice` client extension project using Spring Boot, the Gradle task `bootJar` creates the `.jar` file containing the application and all its dependencies. In this case use the property `fromTask` to trigger the execution of the project’s `bootJar` Gradle task and then include the outputs of the task (i.e., the built `.jar` file) in the root of the resulting LUFFA:

    ```yaml
    assemble:
        - fromTask: bootJar
    ```

    You can execute other commands with `fromTask` as part of your build (like building code written in other programming languages) by defining a [Gradle exec task](https://docs.gradle.org/current/dsl/org.gradle.api.tasks.Exec.html) in your project’s `build.gradle` file.

#### [Example `assemble` Blocks](https://learn.liferay.com/web/guest/w/dxp/building-applications/client-extensions/working-with-client-extensions#example-assemble-blocks) <a href="#example-assemble-blocks" id="example-assemble-blocks"></a>

You can include multiple `from` items in the `assemble` block:

```yaml
assemble:
    - from: build/folder/aaa
      include: "css/*.css"
      into: folder/aaa
    - from: build/folder/bbb
      include: "css/*.css"
      into: folder/bbb
```

You can also include resources from your project that are not built, like

```yaml
assemble:
    - from: assets
      into: static
```

This example places any `*.ico` files from `[project-root]/somewhere/else` into the `static` folder of the LUFFA.

```yaml
assemble:
    - from: somewhere/else
      include: "*.ico"
      into: static
```

For more details on the creation, structure and contents of a LUFFA, see [Packaging Client Extensions](https://learn.liferay.com/web/guest/w/dxp/building-applications/client-extensions/packaging-client-extensions).

### [Deploying to Your Liferay Instance](https://learn.liferay.com/web/guest/w/dxp/building-applications/client-extensions/working-with-client-extensions#deploying-to-your-liferay-instance) <a href="#deploying-to-your-liferay-instance" id="deploying-to-your-liferay-instance"></a>

Client extensions are built into deployable `.zip` archives. Each client extension archive contains a JSON file with the client extension’s settings.

Deploy client extensions by placing the `.zip` files in the correct location for your Liferay installation. The exact commands you’ll use depend on how your Liferay instance is hosted.

#### [Deploying to Liferay SaaS](https://learn.liferay.com/web/guest/w/dxp/building-applications/client-extensions/working-with-client-extensions#deploying-to-liferay-saas) <a href="#deploying-to-liferay-saas" id="deploying-to-liferay-saas"></a>

WARNING

This method does not allow you to deploy client extensions to Liferay PaaS.

To deploy client extensions for Liferay SaaS,

1.  Go to your workspace’s `client-extensions/` folder and run

    ```bash
    ../gradlew clean build
    ```

    The compiled `.zip` files are created in each project’s `dist/` folder. To build one project at a time, run the command from the project’s folder.
2.  Run this command to deploy each client extension to your chosen environment:

    ```bash
    lcp deploy --extension [extension-zip-file]
    ```

    When prompted, select a project and the deployment environment. The zip files are uploaded to your Liferay SaaS project once the command completes.

#### [Deploying to a Self-Hosted Liferay Instance](https://learn.liferay.com/web/guest/w/dxp/building-applications/client-extensions/working-with-client-extensions#deploying-to-a-self-hosted-liferay-instance) <a href="#deploying-to-a-self-hosted-liferay-instance" id="deploying-to-a-self-hosted-liferay-instance"></a>

If you self-host your Liferay installation, use the workspace bundle zip to deploy your client extensions. To build and deploy your client extensions, run this command from your workspace’s `client-extensions/` folder:

```bash
../gradlew clean distBundleZip
```

If you must manually deploy the zip files, run

```bash
../gradlew clean build
```

Then copy the archives from each project’s `dist/` folder into the server’s `[Liferay Home]/osgi/client-extensions/` folder.

### [Context-Sensitive Information](https://learn.liferay.com/web/guest/w/dxp/building-applications/client-extensions/working-with-client-extensions#context-sensitive-information) <a href="#context-sensitive-information" id="context-sensitive-information"></a>

Client extensions are portable: you should never hard-code environment-specific details like domain name, network address, or Liferay’s domains. Client extensions can find context-sensitive information about their context at runtime.

Every client extension workload is provided with a set of _routes_ containing important context-sensitive metadata automatically. With this routes-based approach, application logic can retrieve context sensitive information uniformly, regardless of where it is invoked. You only need to point your client extension projects to it.

#### [Routes](https://learn.liferay.com/web/guest/w/dxp/building-applications/client-extensions/working-with-client-extensions#routes) <a href="#routes" id="routes"></a>

A _route_ is a directory structure containing a set of key/value pairs. File names are the keys, and the file contents are the values. The directory structure is ignored and the directory path is the value of an environment variable. It follows the same pattern as [Kubernetes configMaps](https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-configmap/#add-configmap-data-to-a-volume).

The environment variable you use can point to one of two types of routes:

1.  `LIFERAY_ROUTES_DXP`: The directory path to the route with context-sensitive metadata for the _Liferay virtual instance_ where it’s deployed.

    Here is an example of the `LIFERAY_ROUTES_DXP` route:

    ```properties
    .
    # A newline-separated list of every domain belonging to the DXP virtual instance
    ├── com.liferay.lxc.dxp.domains
    # The primary domain ("Virtual Host" field) of the DXP virtual instance
    ├── com.liferay.lxc.dxp.main.domain
    # The protocol with which to communicate with DXP virtual instance (http or https)
    └── com.liferay.lxc.dxp.server.protocol
    ```
2.  `LIFERAY_ROUTES_CLIENT_EXTENSION`: The directory path to the route which contains context-sensitive metadata for the _client extension project_ itself.

    See [OAuth Headless Server Client Extensions](https://learn.liferay.com/web/guest/w/dxp/building-applications/client-extensions/configuration-client-extensions#oauth-headless-server-client-extensions) and [OAuth User Agent Client Extensions](https://learn.liferay.com/web/guest/w/dxp/building-applications/client-extensions/configuration-client-extensions#oauth-user-agent-client-extensions) for examples.

#### [Pointing to Routes in Liferay Cloud](https://learn.liferay.com/web/guest/w/dxp/building-applications/client-extensions/working-with-client-extensions#pointing-to-routes-in-liferay-cloud) <a href="#pointing-to-routes-in-liferay-cloud" id="pointing-to-routes-in-liferay-cloud"></a>

Containers in Liferay Cloud have these environment variables set automatically. The routes are mounted automatically into the containers at the paths the environment variables define.

#### [Pointing to Routes in Self-Hosted Environments](https://learn.liferay.com/web/guest/w/dxp/building-applications/client-extensions/working-with-client-extensions#pointing-to-routes-in-self-hosted-environments) <a href="#pointing-to-routes-in-self-hosted-environments" id="pointing-to-routes-in-self-hosted-environments"></a>

When using Liferay Workspace’s `Exec`, `JavaExec`, and `NodeExec` Gradle tasks, these environment variables are given default values automatically. It uses these default values:

| **Environment variable**          | **Default value**                                               |
| --------------------------------- | --------------------------------------------------------------- |
| `LIFERAY_ROUTES_DXP`              | `[Liferay Home]/routes/default/dxp`                             |
| `LIFERAY_ROUTES_CLIENT_EXTENSION` | `[Liferay Home]/routes/default/[Client extension project name]` |

The environment variables use the `liferay.workspace.home.dir` property in your Liferay workspace for your Liferay home directory, and `default` to indicate your default Liferay virtual instance. Define these environment variables with a specific virtual instance ID instead of `default` to point to it instead.

NOTE

If you have a Liferay workspace version prior to 9.0.2, you _must_ define these environment variables yourself, following the same form.

These two environment variables must be provided to client extension processes when they are invoked in order to access the metadata.

\
