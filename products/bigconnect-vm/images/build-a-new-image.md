# Build a new image

There are three ways you can build new images.

{% hint style="info" %}
On a multi-node environment, the built image will only be available on the node you select in the **Deployment** section. To make the image available to all nodes, consider adding a registry to BigConnect VM.
{% endhint %}

{% hint style="warning" %}
When building an image with BigConnect VM, you are unable to use `ADD` or `COPY` commands referencing files on the host. We recommend using `wget` or similar to retrieve files from a HTTP/S URL instead
{% endhint %}

## Method 1: Using the web editor

From the menu select **Images** then click **Build a new image**. Next, give the image a descriptive name (you can enter multiple names), select the **Web editor** option under **Build method**, then write your Dockerfile in the web editor.

![](<../../../.gitbook/assets/image (715).png>)

Scroll down and select the node you want to save the image on (if on a multi-node environment) then click **Build the image**.

![](<../../../.gitbook/assets/image (718).png>)

When the build is finished, select the **Output** tab to view the build history and the result.

## Method 2: Uploading a Dockerfile

If you have an existing Dockerfile, you can upload it to BigConnect VM and use it to build the image.

From the menu select **Images** then click **Build a new image**. Next, give the image a descriptive name (you can enter multiple names), select the **Upload** option under **Build method**, then browse to and upload the Dockerfile.

![](<../../../.gitbook/assets/image (745).png>)

Scroll down and select the node you want to save the image on (if on a multi-node environment) then click **Build the image**.

When the build is finished, select the **Output** tab to view the build history and the result.

## Method 3: Providing a Dockerfile from a URL

If the Dockerfile is hosted on the Internet (either in a tarball or a public GitHub repository), you can download it directly to Portainer via its URL.

From the menu select **Images** then click **Build a new image**. Next, give the image a descriptive name (you can enter multiple names), select the **Upload** option under **Build method**, then enter the **URL** of the file and the **Dockerfile path** within the tarball or repository.

![](<../../../.gitbook/assets/image (750).png>)

Scroll down and select the node you want to save the image on (if on a multi-node environment) click **Build the image**.&#x20;

When the build is finished, select the **Output** tab to view the build history and the result.
