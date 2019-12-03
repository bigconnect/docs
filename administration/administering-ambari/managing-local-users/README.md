# Managing local users

As an Ambari administrator, you can create and manage users and groups available to Ambari. You can also import user and group information into Ambari from external LDAP systems.

You use the Ambari Admin page to manage both local and LDAP users. Local users are stored in and authenticate against the Ambari database. LDAP users have basic account information stored in the Ambari database. Unlike local users, LDAP users authenticate against an external LDAP system. To use LDAP users with Ambari, you must configure Ambari to authenticate against an external LDAP system. Ambari grants no permissions by default to a new user, created either locally or by synchronizing against LDAP, You, as an Ambari administrator, must explicitly grant each user permissions to access clusters or views.

You, as an Ambari administrator, can create new users, delete users, change user passwords, and edit user settings.

{% page-ref page="manage-privileges-for-local-and-ldap-users.md" %}

{% page-ref page="create-a-local-user.md" %}

{% page-ref page="set-user-status.md" %}

{% page-ref page="grant-ambari-admin-privileges.md" %}

{% page-ref page="change-the-password-for-a-local-user.md" %}

{% page-ref page="delete-a-local-user.md" %}



  


