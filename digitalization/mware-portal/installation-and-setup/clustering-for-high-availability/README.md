# Clustering for High Availability

MWARE Portal can scale to serve everything from the smallest to the largest web sites. Out-of-the-box, it’s configured optimally for a single server environment. If you expect to have high traffic or want to improve the overall fault tolerance of your site, consider clustering system.

![MWARE Portal is designed to scale to as large an installation as you need.](https://resources.learn.liferay.com/images/dxp/latest/en/installation-and-upgrades/setting-up-liferay/clustering-for-high-availability/images/01.png)

MWARE Portal works well in clusters of multiple machines (horizontal cluster) or in clusters of multiple VMs on a single machine (vertical cluster), or any combination.

### Clustering Requirements <a href="#clustering-requirements" id="clustering-requirements"></a>

Here is what’s required to create an effectively functional DXP cluster.

1. MWARE Portal installed on separate application server nodes
2. A database or database cluster that’s accessible to all nodes
3. A File Store that’s accessible to all nodes
4. A search engine (running outside of the portal) that is optionally clustered
5. Cluster Link configured to enable communication and cache replication across all nodes

NOTE

To ensure there are no concurrency issues and that the master node is selected correctly, start your nodes sequentially.

After you’ve configured your portal cluster, you can deploy applications to it and continue improving user experiences. Please see Maintaining Clustered Installations for more information.

\
