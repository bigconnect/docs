# Export an Ambari Blueprint

You can export a cluster definition as a JSON file for reuse.

An Ambari Blueprint defines the Stack version and service components for a cluster, saved as JSON format file. An Ambari administrator can export a blueprint during cluster installation, or after the cluster has been deployed.

* During cluster installation, in the Ambari Installation wizard **Review** dialog, click **Generate Blueprint**.
* After a cluster has been deployed:
  1. From the Ambari Dashboard user menu, click **Manage Ambari**.
  2. In **Admin &gt; Cluster Information**, click **Download**. A file named blueprint.json exports to the default directory for file downloads on your local machine.

