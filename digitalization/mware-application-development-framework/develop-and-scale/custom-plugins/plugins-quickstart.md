# Plugins quickstart

#### &#x20;Creating a Dev Environment Locally

The CLI provides an all-in-one command for spinning up a local dev environment. This will spin up a fresh server with a setup wizard that will allow you to develop plugins. Let's create a directory where we will store our plugins. You can name this anything that you want, but we are going to call ours `plugins`.

`mkdir plugins`

`cd plugins`

You can then use the CLI command to start the dev environment.

`plugins --dev`

You will see the output below. Since we already navigated to the directory where we will create plugins, we can just hit enter on the `Directory to watch` step, since we are already in there. If you are running the `dev` command in another directory, you will need to point it to the directory where you plan to develop your plugins.

she

```Text
? Directory to watch ./
Starting services, this may take a moment - first time this may take a few minutes to download images.
Recreating plugins_1 ...
Recreating plugins_1 ... done
Services started, please go to http://localhost:10000 for next steps.
Configuration has been written to docker-compose.yaml
Development environment started successfully - connect at: http://localhost:10000
Use the following credentials to login:
Email: admin@admin.com
Password: admin
```

Credentials to your dev instance will be printed to the console and you can then access your instance: `localhost:10000` and log in.



From here, you will be able to access all of your plugins inside the `plugins` directory.

<figure><img src="https://files.readme.io/cd7daec0a8675566a41e31e08a98f0d7ec89b34fe8c7ed6dde2eb64f8344567b-Screenshot_2024-10-22_at_16.01.59.png" alt=""><figcaption></figcaption></figure>

To get started with creating a plugin or a datasource, follow the specific guides.
