# Service users and groups

Ambari creates Unix accounts for each service user and group during cluster installation.

Each Hadoop service runs under the ownership of their respective Unix accounts. These accounts are known as service users. These service users belong to a special Unix group. "Smoke Test" is a service user dedicated specifically for running smoke tests on components during installation using the Services tab of the Ambari Web UI. You can also run service checks as the Smoke Test user on-demand after installation. You can customize any of these users and groups using the Misc tab during the Customize Services installation step.

Use the **Skip Group Modifications** option to not modify the Linux groups in the cluster. Choosing this option is typically required if your environment manages groups using LDAP and not on the local Linux machines.

If you choose to customize names, Ambari checks to see if these custom accounts already exist. If they do not exist, Ambari creates them. The default accounts are always created during installation whether or not custom accounts are specified. These default accounts are not used and can be removed post-install.

All new service user accounts, and any existing user accounts used as service users, must have a UID greater than or equal to 1000.

{% page-ref page="default-user-accounts.md" %}

{% page-ref page="default-group-accounts.md" %}

{% page-ref page="update-dependent-properties.md" %}

