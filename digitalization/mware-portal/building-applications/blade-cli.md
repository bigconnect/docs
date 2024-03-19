# Blade CLI

Below is a summary of the available blade commands. Help is available at the CLI by typing `blade help [command]`.

| Command               | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| --------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `convert`             | Converts a Plugins SDK plugin project to a Gradle Workspace project.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| `create`              | Creates a new Liferay project from available templates.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| `deploy`              | Builds and deploys projects to Liferay.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| `extension install`   | Installs an extension into Blade CLI.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| `extension uninstall` | Uninstalls an extension from Blade CLI.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| `gw`                  | Executes a Gradle command using the Gradle Wrapper, if detected (e.g., `blade gw tasks`).                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| `help`                | Provides information for Blade CLI’s commands.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| `init`                | Initializes a new Liferay Workspace.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| `samples`             | Generates a sample project.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| `server init`         | Initializes the Liferay server configured in Liferay Workspace’s `gradle.properties` file. Set the `liferay.workspace.bundle.url` property to configure the server to initialize.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| `server start`        | Starts the Liferay server in the background. You can add the `-d` flag to start the server in debug mode. Debug mode can be customized by adding the `-p` tag to set the custom remote debugging port (defaults are `8000` for Tomcat and `8787` for WildFly) and/or the boolean `-s` tag to set whether you want to suspend the started server until the debugger is connected.                                                                                                                                                                                                                                                                                                                                                        |
| `server stop`         | Stops the Liferay server.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| `server run`          | Starts the Liferay server in the foreground. See the `server start` property for more information.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| `sh`                  | Connects to Liferay DXP, executes a Gogo command, and returns the output. For example, `blade sh lb` lists all bundles using the Gogo shell.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| `update`              | Updates Blade CLI to the latest version.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| `upgradeProps`        | Analyzes your old `portal-ext.properties` and your newly installed 7.x server to show you properties moved to OSGi configuration files or removed from the product.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| `watch`               | Watches for changes to a deployed project and automatically redeploys it when changes are detected. This command does not rebuild your project and copy it to your installation every time a change is detected, but instead installs it into the runtime as a reference. This means that Liferay DXP does not make a cached copy of the project, so it can see changes that are made to your project’s files immediately. When you cancel the `watch` task, your module is uninstalled automatically. The `blade deploy -w` command works similarly to `blade watch`, except it manually recompiles and deploys your project every time a change is detected. This is slower, but it preserves your deployed project between restarts. |
| `version`             | Displays version information about Blade CLI.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |

## [Generating Projects with Blade CLI](https://learn.liferay.com/web/guest/w/dxp/building-applications/tooling/blade-cli/generating-projects-with-blade-cli#generating-projects-with-blade-cli) <a href="#generating-projects-with-blade-cli" id="generating-projects-with-blade-cli"></a>

Blade CLI exists to create, build, and deploy Liferay projects in Liferay Workspaces. Once created, these projects can be imported into an IDE or worked on directly. Here you’ll learn the various ways in which you can create and manage Liferay projects.

### [Creating a Liferay Workspace](https://learn.liferay.com/web/guest/w/dxp/building-applications/tooling/blade-cli/generating-projects-with-blade-cli#creating-a-liferay-workspace) <a href="#creating-a-liferay-workspace" id="creating-a-liferay-workspace"></a>

Liferay Workspace is a set of folders on your machine where you store projects and DevOps configurations. Liferay Workspaces can be imported into any IDE, because they are Gradle projects. See [Liferay Workspace](https://learn.liferay.com/web/guest/w/dxp/building-applications/tooling/liferay-workspace/what-is-liferay-workspace) for further information. To create a Liferay Workspace follow these steps,

1. In your CLI, navigate to the folder where you want to create a Liferay Workspace.
2.  Run this command:

    ```bash
    blade init -v 7.3 [workspace name]
    ```

### [Creating a Project](https://learn.liferay.com/web/guest/w/dxp/building-applications/tooling/blade-cli/generating-projects-with-blade-cli#creating-a-project) <a href="#creating-a-project" id="creating-a-project"></a>

Projects exist in a Liferay Workspace. Here are the important options to remember:

**-t:** Specify the project template to use. You can get a list of these by typing `blade create -l`.

**-p:** Specify the package name for your class.

**-c:** Specify your class name.

**-v:** Specify the Liferay version; for example, `7.1`, `7.2`, `7.3`, or `7.4`. Note: you can omit this option and Blade defaults to the version configured in Workspace.

Putting these together, if you want to create a Liferay MVC Portlet called “guestbook,” use this command:

```bash
blade create -t mvc-portlet -p com.acme.z3x1.portlet -c GuestbookPortlet guestbook
```

This creates a project using the MVC Portlet template, a default package called `com.acme.z3x1.portlet`, a portlet class called `GuestbookPortlet`, for Liferay 7.4, called _guestbook_. You can now import your Workspace into your IDE. The Liferay IntelliJ plugin or the Eclipse plugins in Liferay Developer Studio provide enhanced support. Note that if you create a new Workspace in your project with Blade, you must refresh your Gradle project in your IDE.

### [Creating Sample Projects](https://learn.liferay.com/web/guest/w/dxp/building-applications/tooling/blade-cli/generating-projects-with-blade-cli#creating-sample-projects) <a href="#creating-sample-projects" id="creating-sample-projects"></a>

Liferay maintains a GitHub repository of [sample projects](https://github.com/liferay/liferay-blade-samples/tree/7.4). These are fully-implemented samples of various Liferay technologies you can use as a starting point for your projects. Rather than clone the repository to get access to them, however, you can create them locally using Blade CLI.

1.  Find the sample project you want:

    ```bash
    blade samples
    ```
2.  Say you want a working example of a [model listener](https://learn.liferay.com/web/guest/w/dxp/liferay-internals/extending-liferay/creating-a-model-listener). Type this command:

    ```bash
    blade samples model-listener
    ```
3.  If you want a specific version of the sample, you can pass in a version:

    ```bash
    blade samples -v 7.1 model-listener
    ```

### [Converting Legacy Plugins SDK Projects](https://learn.liferay.com/web/guest/w/dxp/building-applications/tooling/blade-cli/generating-projects-with-blade-cli#converting-legacy-plugins-sdk-projects) <a href="#converting-legacy-plugins-sdk-projects" id="converting-legacy-plugins-sdk-projects"></a>

If you have Liferay projects prior to version 7.0, they are in a Plugins SDK. To use them with any version of Liferay beyond 6.2, you must migrate them from the Plugins SDK to a Liferay Workspace.

1. Create a [Liferay Workspace](https://learn.liferay.com/web/guest/w/dxp/building-applications/tooling/blade-cli/generating-projects-with-blade-cli#creating-a-liferay-workspace) if you haven’t already.
2.  From within the Liferay Workspace, execute this command:

    ```bash
    blade convert -s [path to old Plugins SDK] -a
    ```

    This converts all projects in the Plugins SDK to Workspace projects.
3.  If you want to convert only a single project, use this command instead:

    ```bash
    blade convert -s [path to old Plugins SDK] [name of Plugins SDK project to convert]
    ```

* When converting a project containing Service Builder services, Blade CLI creates separate API and service OSGi modules. The portlet remains a WAR and moves into the `wars` folder.
* Themes are converted to leverage NodeJS like Liferay 7.x themes. To convert a Java-based theme, add the `-t` option, which uses the Theme Builder Gradle plugin instead.

\
