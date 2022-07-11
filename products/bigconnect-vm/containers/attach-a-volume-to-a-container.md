# Attach a volume to a container

{% hint style="warning" %}
This article explains how to attach a new volume to a running container. This operation destroys the running container and starts a new one with the volume attached.

**Always back up your data before running this operation.**
{% endhint %}

From the menu select **Containers**, select the container that you want to attach a volume to, then click **Duplicate/Edit**.&#x20;

Scroll down to **Advanced container settings**. Click **Volumes** then click **map additional volume**.

![](<../../../.gitbook/assets/image (711).png>)

In the **container** field enter the path. In the **volume** field enter the volume to attach to the container.

![](<../../../.gitbook/assets/image (712).png>)

When you're ready, click **Deploy the container**. When the confirmation message appears, click **Replace**.
