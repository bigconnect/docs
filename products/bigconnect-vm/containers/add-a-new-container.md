# Add a new container

Select **Containers** from the menu then click **Add container**.

![](<../../../.gitbook/assets/image (763).png>)

{% hint style="info" %}
When using Docker Hub you can use the **Search** button to search for the image you have entered, and ensure that you have the correct name and tag. Portainer also displays the number of pulls remaining for your Docker Hub account when using an anonymous account.
{% endhint %}

Alternatively you can switch to advanced mode to manually enter registry and image details. This is useful if you want to do a one-off container deployment from a registry that isn't configured within BigConnect VM.

![](<../../../.gitbook/assets/image (780).png>)

## Network ports configuration section <a href="#network-ports-configuration-section" id="network-ports-configuration-section"></a>

![](<../../../.gitbook/assets/image (762).png>)

**Publish all exposed network ports to random host ports**

Toggle on to allow BigConnect VM to randomly assign ports on the host to the exposed ports in the container.

**Manual network port publishing**

Click **publish a new network port** to create manual port mappings for the contain

## Actions section <a href="#actions-section" id="actions-section"></a>

![](<../../../.gitbook/assets/image (710).png>)

Toggle this option on to automatically remove the container once it exits. This is useful if you want to run a container only once.

## Advanced container settings

Choose from a range of options to customize the deployment.

![](<../../../.gitbook/assets/image (774).png>)
