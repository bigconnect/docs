# Inspect or edit a stack

## Inspecting a stack

From the menu select **Stacks** then select the stack you want to inspect. From here you can stop, delete or create a template from the stack, and if deployed from Git you can detach the stack from the Git repository.

![](<../../../.gitbook/assets/image (712) (1).png>)

If the stack was deployed from a Git repository, you can:

* Configure automatic updates or manually pull and redeploy the stack.
* View and edit the stack's environment variables.

![](<../../../.gitbook/assets/image (704).png>)

If the stack was deployed using the Web Editor or uploaded, you will have the option to edit your compose file manually.

Regardless of the deployment method used, you can also migrate or duplicate the stack.

### Docker Standalone

When using Docker Standalone, you can:

* View the containers that make up the stack.
* Check to see if they are running or stopped.
* Get access to logs.
* Inspect individual containers.
* View container statistics.
* Get access to the container's console.

![](<../../../.gitbook/assets/image (735) (1).png>)

### Docker Swarm

When using Docker Swarm, you can:

* View the services that make up the stack.
* Check to see if they are running or stopped.
* See how many replicas are running on each host.
* Get access to logs.
* Inspect individual services.
* View service statistics.
* Get access to the service's console.

![](<../../../.gitbook/assets/image (726).png>)

## Editing a stack

Editing a stack allows you to make changes to the configuration and redeploy those changes. To edit a stack, from the menu select **Stacks**, select the stack you want to edit, then select the **Editor** tab.

{% hint style="info" %}
The Editor tab is only available for stacks that were deployed using the Web Editor. For stacks deployed from a Git repository, the docker-compose file must be edited in the repository itself.
{% endhint %}

![](<../../../.gitbook/assets/image (741) (1).png>)

Here, you can edit the Compose file for the stack to suit your needs, as well as toggle the stack webhook and retrieve the webhook URL. You can also make changes to environment variables for the stack, and on Docker Swarm environments you can prune services if you have made changes that remove some services from the stack.

When you have finished making changes, click **Update the stack**.
