# Understanding group types

Ambari supports two types of groups: local and LDAP.

You use the Ambari Admin page to manage both local and LDAP groups. Local groups are stored in the Ambari database. LDAP groups have basic information stored in the Ambari database, including group membership information. Unlike local groups, LDAP groups are imported and synchronized from an external LDAP system. To use LDAP groups with Ambari, you must configure Ambari to authenticate against an external LDAP system. Ambari grants no permissions by default to a new group, created either locally or by synchronizing against LDAP.

