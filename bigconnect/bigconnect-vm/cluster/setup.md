# Setup

Under **Setup**, you can make changes to your environment, enabling and disabling features and security settings.

### Host and Filesystem <a href="#host-and-filesystem" id="host-and-filesystem"></a>

This section is where you configure how BigConnect VM interacts with elements of the host.

For security, these features are disabled by default. Be sure that you understand their impact before enabling them.

![](https://docs.portainer.io/\~gitbook/image?url=https:%2F%2Fcontent.gitbook.com%2Fcontent%2FtLcRoAdw9BYwwpba4ZAD%2Fblobs%2F75gRcBvUDrcb84rWWbiF%2F2.15-docker\_hosts\_features\_config.png\&width=768\&dpr=4\&quality=100\&sign=6dc7ab9ad7db6909f996be75c45a68b2622c399d5802f05a74f106c030da9a3e)

| Option                                          | Overview                                                                                                                                                                         |
| ----------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Enable host management features                 | Enabling host management features allows you to see the available devices and storage on the physical nodes across your cluster as well as browse the nodes' filesystems.        |
| Enable volume management for non-administrators | Enabling this feature allows non-administrator users to manage volumes on an environment. If this is disabled, users below administrator level have read-only access to volumes. |

### Change Window Settings <a href="#change-window-settings" id="change-window-settings"></a>

This setting allows you to specify a window within which GitOps updates to your applications can be applied.

If this setting is enabled and an update is made to an application outside of this window, it will not be applied.

![](https://docs.portainer.io/\~gitbook/image?url=https:%2F%2F833443324-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FtLcRoAdw9BYwwpba4ZAD%252Fuploads%252FIcH6QTNBhS9fgjLGOMYX%252F2.19-kubernetes-cluster-setup-changewindow.png%3Falt=media%26token=e6b61597-5256-4359-a056-6cbc61a64406\&width=768\&dpr=4\&quality=100\&sign=73f8246879f0b1db5a233ebd69ad20e6c2fe7ad66145323b10f53f17c8d3211c)

### Docker Security Settings <a href="#docker-security-settings" id="docker-security-settings"></a>

This section allows you to toggle assorted Docker-related security settings for an environment.

![](https://docs.portainer.io/\~gitbook/image?url=https:%2F%2Fcontent.gitbook.com%2Fcontent%2FtLcRoAdw9BYwwpba4ZAD%2Fblobs%2F9djFN4pTV5KlwAbY7icI%2F2.15-docker\_hosts\_security\_settings.png\&width=768\&dpr=4\&quality=100\&sign=1e909b1c82f6fef238baef86b1323443af6a1672de772a935d4e685fe1a6ece8)

| Option                                                | Overview                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| ----------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Disable bind mounts for non-administrators            | Prevents non-admin users within BigConnect VM from using bind mounts when creating containers and/or services/stacks. When toggled on, the option to attach to a host file system path is removed.                                                                                                                                                                                                                                                                                                                                                              |
| Disable privileged mode for non-administrators        | Prevents non-admin users from elevating the privilege of a container to bypass SELinux/AppArmor. When toggled on, the option to select **Privileged** mode when adding a container is removed.                                                                                                                                                                                                                                                                                                                                                                  |
| Disable the use of host PID 1 for non-administrators  | Prevents non-admin users from requesting that a deployed container operates as the host PID. This is a security risk if used by a non-trustworthy authorized user because when they operate as PID1, they are in effect able to run any command in the container console as root on the host.                                                                                                                                                                                                                                                                   |
| Disable the use of Stacks for non-administrators      | This is a 'sledgehammer' approach to removing any possibility for non-admin users within BigConnect VM to find and use weaknesses in the Docker architecture. Whilst BigConnect VM has the ability to disable some of the more common exploits, we cannot possibly block them all because there are any number of capabilities that could be added to a container to attempt to gain access to the host. This feature simply allows an admin to disable all possible entry points.                                                                              |
| Disable device mappings for non-administrators        | Blocks users from mapping host devices into containers. Whilst the ability to map devices is generally used for good (e.g. mapping a GPU into a container), it can equally be used by non-trustworthy authorized users to map a physical storage device into a container. It is possible to mount `/dev/sda1` into a container, and then from a console of that container, the user would have complete access to the sda1 device without restriction. By toggling this on, BigConnect VM blocks the ability for non-admins to map ANY devices into containers. |
| Disable container capabilities for non-administrators | Toggle on to hide the **Container capabilities** tab for non-administrators when they are adding a container.                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| Disable sysctl settings for non-administrators        | Toggle on to stop non-admin users from using sysctl options, preventing them from recreating, duplicating or editing containers.                                                                                                                                                                                                                                                                                                                                                                                                                                |

### Other <a href="#other" id="other"></a>

This section contains other assorted environment-specific settings.

![](https://docs.portainer.io/\~gitbook/image?url=https:%2F%2Fcontent.gitbook.com%2Fcontent%2FtLcRoAdw9BYwwpba4ZAD%2Fblobs%2FmMdrmFBQxrHf9oxg1XaS%2F2.18-swarm-setup-other.png\&width=768\&dpr=4\&quality=100\&sign=34ae5fb1c867cfb8eb573c26beb722f06017d251186aa2c831eca66227e2eb11)

| Option                                                                    | Overview                                                                                                        |
| ------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------- |
| Show GPU in the UI                                                        | GPU functionality is currently not available in Docker Swarm environments so this toggle is disabled.           |
| Show an image(s) up to date indicator for Stacks, Services and Containers | Toggle on to enable the new image notification feature for this environment. Toggle off to disable the feature. |
