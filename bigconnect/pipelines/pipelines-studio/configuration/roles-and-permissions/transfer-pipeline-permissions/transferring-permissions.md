# Transferring Permissions

You can transfer all permissions from a user or group to another user or group.

To change the permissions related to individual pipelines, configure the sharing properties for each pipeline. For more information, see Sharing Pipelines.

1. To transfer permissions, click the **Administration** icon and then click **Transfer Permissions**.\
   The T**ransfer User & Group Permissions** dialog box displays.
2. If users or groups are listed, review the list and correct the suggested mappings as needed.\
   \
   On the left side of the dialog box, BigConnect Pipelines lists any users or groups that no longer exist but still have pipeline permissions. This allows you to easily transfer permissions from those users and groups.\
   \
   The right side of the dialog box displays the list of available users and groups. Select the user or group to receive the permissions for each mapping.\
   \
   Or, if you do not want to transfer permissions from a listed user or group, use the **Subtract** icon to remove the row.
3. Using simple or bulk edit mode, click the **Add** icon to add a row.
4. On the left side, enter the name of the user or group that you want to transfer permissions from.
5. On the right side, select the user or group to receive those permissions.\
   \
   Pipeline ownership can only be transferred to another user. If you transfer permissions from a user to a group and the user is also a pipeline owner, the user retains ownership. A user with the Admin role can transfer the pipeline ownership to another user, as needed.\
   \
   **Note**: You can transfer permissions from a user or group to a single user or group. If you map the same user or group to multiple targets, BigConnect Pipelines transfers permission to the last user or group in the list.
6. Add as many rows and mappings as needed, then to save your changes, click **Update**.\
   \
   The permission changes take effect immediately.
