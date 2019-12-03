# Using Ambari Blueprints

Ambari Blueprints provide an API to perform cluster installations.

You can build a reusable "blueprint" that defines which Stack to use, how Service Components should be laid out across a cluster and what configurations to set.

![](../../../.gitbook/assets/image%20%2815%29.png)

After setting up a blueprint, you can call the API to instantiate the cluster by providing the list of hosts to use. The Ambari Blueprint framework promotes reusability and facilitates automating cluster installations without UI interaction.

**Related information**

[Ambari Wiki - Blueprints](https://cwiki.apache.org/confluence/display/AMBARI/Blueprints)

