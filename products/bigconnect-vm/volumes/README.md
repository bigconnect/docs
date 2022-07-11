# Volumes

A volume is a data storage area that can be mounted into a container to provide persistent storage. Unlike bind mounts, volumes are independent of the underlying OS and are fully managed by the Docker Engine.

![](<../../../.gitbook/assets/image (765).png>)

A volume with the **external** flag was created outside of BigConnect VM, which means BigConnect VMhas limited knowledge on it compared to one created within BigConnect VM. A label of **unused** means that BigConnect VMcannot see any applications that are using this volume. This label may also appear on **external** resources because of the limited information available.

In BigConnect VM you can view a list of the volumes on your environment, add new volumes and remove existing volumes.
