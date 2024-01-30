# Add a new volume

## Adding a local volume

From the menu select **Volumes** then click **Add volume**.&#x20;

![](<../../../.gitbook/assets/image (322).png>)

When you're finished, click **Create the volume**.

## Adding an NFS volume

{% hint style="info" %}
In BigConnect VM, you can mount an NFS volume to persist the data of your containers.
{% endhint %}

From the menu select **Volumes** then click **Add volume**.&#x20;

![](<../../../.gitbook/assets/image (338).png>)

Under the **NFS Settings** section, complete the following.

![](<../../../.gitbook/assets/image (353).png>)

When you're finished, click **Create the volume**.

## Adding a CIFS volume

{% hint style="info" %}
In BigConnect VM, you can mount a CIFS volume to persist the data of your containers.
{% endhint %}

From the menu select **Volumes** then click **Add volume**.

![](<../../../.gitbook/assets/image (317).png>)

Under the **CIFS Settings** section, complete the following:

![](<../../../.gitbook/assets/image (367).png>)

When you're finished, click **Create the volume**.

## Adding a tmpfs volume

From the menu select **Volumes** then click **Add volume**.

Complete the information in the **Create volume** screen, using the table below as a guide.

![](<../../../.gitbook/assets/image (370).png>)

When you're finished, click **Create the volume**. The volume can now be attached to a container in the same way as any other volume.

![](<../../../.gitbook/assets/image (311).png>)

Once attached, you can confirm that the tmpfs volume has been mounted correctly within the container:

![](<../../../.gitbook/assets/image (379).png>)
