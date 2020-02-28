# Terminology

Familiarity with the following basic terms can help you to understand the key concepts associated with Ambari administration:

**Ambari Admin**  
Specific privileges granted to a user that enables that user to administer Ambari. Users with the Ambari Admin privilege can grant this privilege to other users, or revoke it from them.

**Ambari Admin Page**  
Ambari Web page accessible only to users with the Ambari Admin privilege.

**Ambari Web**   
Graphical User Interface \(GUI\) that provides user access to Ambari-managed cluster resources.

**account**  
User name, password, and privileges.

**cluster**   
An installation of a Hadoop cluster, based on a particular stack, that is managed by Ambari.

**group**   
Unique group of users in Ambari.

**group type**  
Local and LDAP. Local groups are maintained in the Ambari database. LDAP groups are imported to \(and synchronized with\) an external LDAP, if one is configured.

**permissions**   
The permissions granted to a principal user or group for a particular view.

**principal**   
User or group that can be authenticated by Ambari.

**privilege**   
The mapping of a principal to a permission or role and a resource. For example, the user joe.operator is granted the role of Cluster Operator on the cluster DevCluster.

**resource**   
The resource available and managed in Ambari. Ambari supports two types of resources: cluster and view. An Ambari Admin assigns permissions for a resource for users and groups.

**role**   
The role that is assigned to a principal \(user or group\) on a particular cluster.

**user**   
Unique user in Ambari.

**user type**   
Local and LDAP. Local users are maintained in the Ambari database and authentication is performed against the Ambari database. LDAP users are imported to \(and synchronized with\) an external LDAP, if one is configured.

**version**   
Stack version, which includes a set of repositories to install that version on a cluster.

**view**   
A user interface component that is available to Ambari.

