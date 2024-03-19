# Clustering for High Availability

Liferay DXP can scale to serve everything from the smallest to the largest web sites. Out-of-the-box, it’s configured optimally for a single server environment. If you expect to have high traffic or want to improve the overall fault tolerance of your site, consider clustering DXP.

![Liferay DXP is designed to scale to as large an installation as you need.](https://resources.learn.liferay.com/images/dxp/latest/en/installation-and-upgrades/setting-up-liferay/clustering-for-high-availability/images/01.png)

Liferay DXP works well in clusters of multiple machines (horizontal cluster) or in clusters of multiple VMs on a single machine (vertical cluster), or any combination.

### [Clustering Requirements](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/setting-up-liferay/clustering-for-high-availability#clustering-requirements) <a href="#clustering-requirements" id="clustering-requirements"></a>

Here is what’s required to create an effectively functional DXP cluster.

1. Liferay DXP [installed](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/installing-liferay) on separate application server nodes
2. A [database or database cluster](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/setting-up-liferay/clustering-for-high-availability/database-configuration-for-cluster-nodes) that’s accessible to all nodes
3. A [File Store](https://learn.liferay.com/web/guest/w/dxp/system-administration/file-storage) that’s accessible to all nodes
4. A [search engine (running outside of DXP) that is optionally clustered](https://learn.liferay.com/web/guest/w/dxp/using-search/installing-and-upgrading-a-search-engine/installing-a-search-engine)
5. [Cluster Link configured to enable communication and cache replication across all nodes](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/setting-up-liferay/clustering-for-high-availability/configuring-cluster-link)

NOTE

To ensure there are no concurrency issues and that the master node is selected correctly, start your nodes sequentially.

After you’ve configured your DXP cluster, you can deploy applications to it and continue improving user experiences. Please see [Maintaining Clustered Installations](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/maintaining-a-liferay-installation/maintaining-clustered-installations) for more information.

\
