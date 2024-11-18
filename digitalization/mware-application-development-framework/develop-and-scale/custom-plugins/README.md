# Custom plugins

Mware Application Development Framework allows no-code users to build apps quickly, with more functionality available with a little bit of inline code. Despite this, not every conceivable scenario can be covered by default - but that's where custom plugins come in!

Developers can write their own plugins for where custom datasources and components are needed. Some software development experience is required, however, the process for building plugins is designed to be streamlined and sleekly integrated into the platform.

### Importing plugins

An admin user can see a list of your plugins under the _Plugins_ section of the Portal. To get started using custom plugins, click "Add Plugin". Alternatively you can check our curated list of plugins, including data sources and components, contributed by the community.

\


<figure><img src="https://files.readme.io/bc159e1cee248702843f218a487b0d1720e90a6f83693efde29107a89440f109-Screenshot_2024-10-22_at_11.07.05.png" alt=""><figcaption></figcaption></figure>

You can import a plugin by pressing the `Add plugin` button. This will display a dialog box from which you choose a _Source_.

| Source name | Expected input                                                                                                                                                               |
| ----------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| URL         | <p>A URL path to a tarball, e.g. <code>.tar.gz</code> file.<br><br><em>Allows authentication headers to be added if needed.</em></p>                                         |
| NPM         | The URL of an NPM package of a plugin                                                                                                                                        |
| Github      | <p>The URL of a Github repository, that has a tarball asset in the latest release.<br><br><em>Allows a Github Token to be added for accessing private repositories.</em></p> |
| File Upload | Allows an admin user to directly upload a plugin tarball.                                                                                                                    |

<figure><img src="https://files.readme.io/89da718044369908e89362efa4765e39d1b33bbafe8d7a555edeea86669e6c5c-Screenshot_2024-10-22_at_11.12.51.png" alt=""><figcaption></figcaption></figure>

Once your plugin has been imported, you will now be able to make use of it within all of your apps!

In the case of a **custom datasource**, it will appear as a new datasource at the bottom of the _Add datasource_ dialog and can be used just like any other [data source](https://docs.budibase.com/docs/data-sources).

<figure><img src="https://files.readme.io/8d0ca798008906f1833a44f8941be78985fbcd9c3ef249a2ce6d62cc124a7108-CustomDataSource_1.png" alt=""><figcaption></figcaption></figure>

In the case of a **custom component**, it will appear in the builder components list under the _Plugins_ section.

<figure><img src="https://files.readme.io/5a44802781096d8786e9ece5566659f3fc0ddc1654821da8038104a53f51e1cd-Screenshot_2024-10-22_at_11.34.06.png" alt=""><figcaption></figcaption></figure>

> ### 🚧Cloud hosting
>
> While custom components can also be imported in the cloud environment, custom datasources can only be used in a self-hosted environment, for security reasons.

\


#### Hot reloading (developers only)

You must provide a path to your plugins folder so that the system knows where to import the plugins that you update while developing.

There are three main approaches to this, with the handiest being to run it via the [CLI](https://docs.budibase.com/docs/budibase-cli-setup). Alternatively, you can run it via Docker compose or a local repository.

**CLI**

Make sure you have installed the latest CLI, and initialize it.

If it is already running, stop the service using the command:

`hosting --stop`

Add the plugins directory path using the command:

`hosting --watch-plugin-dir /path-to-your-plugins-directory`

Finally, run `hosting --start` and then do a `yarn watch` within your plugin repo.

**Docker compose**

Make sure you have installed the latest CLI, and initialize it.

Add the plugins directory path (**case sensitive!**) to the **app-service** volumes of your docker compose file:

Yaml

```yaml
services:
  app-service:
    volumes:
      - /Users/<username>/Documents/MyPlugins:/MyPlugins
```

Next, update your `.env` file to include the relative plugins directory, e.g. `PLUGINS_DIR=/MyPlugins`

> ### 📘
>
> The `.env` file will be hidden within the same folder as your `docker-compose.yaml`.\
> Showing hidden files on [Windows](https://support.microsoft.com/en-gb/windows/show-hidden-files-0320fe58-0117-fd59-6851-9b7f9840fdb2).\
> On macOS, press `Cmd+Shift+.` within the folder.

Finally, run `hosting --start` and then do a `yarn watch` within your plugin repo.

**Repo**

Assuming you have cloned the Repo and have it running locally, navigate to `packages/server/.env` and add the following variable:

`PLUGINS_DIR=/Users/<username>/Documents/MyPlugins`

Naturally, you should replace the value with the path to your plugins folder.

If your server is already running, you will need to stop it and then start it with another `yarn dev` so that the new environment variable is loaded.

You will also need to make sure you have run `yarn watch` in your plugin repo. See the custom component and datasource pages for more information on development.

### Updating a plugin

To update an existing plugin, simply import a plugin with a matching name.

Currently, the versioning must be handled manually, where you can have multiple folders for each version of your plugin.

### Deleting a plugin

Click on one of the plugins from the plugins list. This will pop up a dialog from which you can press `Delete`

<figure><img src="https://files.readme.io/8e68de2-Screenshot_2022-09-15_at_13.49.07.png" alt=""><figcaption></figcaption></figure>
