# Add a new stack

## Option 1: Web editor

From the menu select **Stacks**, click **Add stack**, give the stack a descriptive name then select **Web editor**. Use the web editor to define the services.

As part of the stack creation you can enable a stack webhook, allowing you to remotely trigger redeployments of the stack from your repository, for example.

![](<../../../.gitbook/assets/image (743) (1).png>)

As an optional step, you can also use the web editor to define environment variables. You can use these to define values in your compose file that would vary between deployments (for example, hostnames, database names, etc).

Environment variables can be set individually within Portainer or you can use **Load variables from .env file** to upload a file containing your environment variables. Environment variables you define (either individually or via a .env file) will be available to use in your compose file using an `environment` definition:

```
environment:
  - MY_ENVIRONMENT_VARIABLE: ${MY_ENVIRONMENT_VARIABLE}
```

Alternatively, on Docker Standalone environments you can add `stack.env` as an `env_file` definition to add all the environment variables that you have defined individually as well as those included in an uploaded .env file:

```
env_file:
  - stack.env
```

**Note:** Using `env_file` to define a file does not work in Docker Swarm due to the lack of `env_file` support in `docker stack deploy` (used on Swarm environments to deploy your stack). On Docker Swarm, you will need to define each environment variable manually.&#x20;

![](<../../../.gitbook/assets/image (708) (1).png>)

When you're ready, click **Deploy the stack**.

## Option 2: Upload

In Portainer you can create stacks from Compose YML files. To do this, from the menu select **Stacks**, click **Add stack**, then give the stack a descriptive name. Select **Upload** then select the Compose file from your computer.&#x20;

As part of the stack creation you can enable a stack webhook, allowing you to remotely trigger redeployments of the stack from your repository, for example.

![](<../../../.gitbook/assets/image (761) (1).png>)

As an optional step, enter any environment variables. You can use these to define values in your compose file that would vary between deployments (for example, hostnames, database names, etc).

Environment variables can be set individually within Portainer or you can use **Load variables from .env file** to upload a file containing your environment variables. Environment variables you define (either individually or via a .env file) will be available to use in your compose file using an `environment` definition:

```
environment:
  - MY_ENVIRONMENT_VARIABLE: ${MY_ENVIRONMENT_VARIABLE}
```

Alternatively, you can add `stack.env` as an `env_file` definition to add all the environment variables that you have defined individually as well as those included in an uploaded .env file:

```
env_file:
  - stack.env
```

![](<../../../.gitbook/assets/image (716) (1).png>)

When you're ready click **Deploy the stack**.

## Option 3: Git repository

If your Compose file is hosted in a Git repository, you can deploy from there. From the menu select **Stacks**, click **Add stack**, then give the stack a descriptive name. Select **Git Repository** then enter information about your Git repo.

![](<../../../.gitbook/assets/image (757) (1).png>)
