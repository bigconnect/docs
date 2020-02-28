# Sizing

As you create the architecture of your cluster, you will need to allocate service roles among the hosts in the cluster to maximize your use of resources. When multiple roles are assigned to hosts, add together the total resource requirements \(memory, CPUs, disk\) for each role on a host to determine the required hardware.

### Accumulo

| Component | Minimum Memory | Min CPU Cores | Disks |
| :--- | :--- | :---: | :--- |
| **Master** | Minimum: 1 GiB  100-1000 tablets: 4 GB  10,000 or more tablets with 200 or more Tablet Servers: 8 GB  10,000 or more tablets with 300 or more Tablet Servers: 12 GB | 2 Cores. Add more for large clusters or bulk load. | 1 for log files |
| **Tablet Server** | Minimum: 512 MB Java Heap  Recommended: 8 GB  Medium scale production \(200 or more tablets per Tablet Server\): 16 GB | 4 dedicated | 4 or more spindles for each HDFS DataNode  1 disk for local logs \(can be shared with the operating system and/or other Hadoop logs\) |
| **Tracer** | 1 - 2 GB, depending on cluster workloads | 2 or more dedicated | 1 disk for local logs \(can be shared with the operating system and/or other Hadoop logs\) |
| **GC** | 1-2 GB | 2 or more dedicated | 1 disk for local logs \(can be shared with the operating system and/or other Hadoop logs\) |
| **Monitor** | 1-2 GB | 2 or more dedicated | 1 disk for local logs \(can be shared with the operating system and/or other Hadoop logs\) |

### Alluxio

<table>
  <thead>
    <tr>
      <th style="text-align:left">Component</th>
      <th style="text-align:left">Minimum Memory</th>
      <th style="text-align:center">Min CPU Cores</th>
      <th style="text-align:left">Disks</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><b>Master</b>
      </td>
      <td style="text-align:left">
        <p>1 GB</p>
        <p>
          <br />Add an additional 1 GB for each additional 1,000,000 files</p>
      </td>
      <td style="text-align:center">4</td>
      <td style="text-align:left">1 for log files</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Worker</b>
      </td>
      <td style="text-align:left">4 GB (recommended)</td>
      <td style="text-align:center">2</td>
      <td style="text-align:left">1 for log files</td>
    </tr>
  </tbody>
</table>### Ambari

<table>
  <thead>
    <tr>
      <th style="text-align:left">Component</th>
      <th style="text-align:left">Minimum Memory</th>
      <th style="text-align:center">Min CPU Cores</th>
      <th style="text-align:left">Disks</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><b>Server</b>
      </td>
      <td style="text-align:left">
        <p>1-10 managed hosts: 1 GB</p>
        <p>50 managed hosts: 4GB</p>
        <p>100-300 managed hosts: 4GB</p>
        <p>500 managed hosts: 8GB</p>
        <p>1000 managed hosts: 12GB</p>
        <p>2000 managed hosts: 16GB</p>
      </td>
      <td style="text-align:center">4</td>
      <td style="text-align:left">10 - 500 GB</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Agent</b>
      </td>
      <td style="text-align:left">1GB</td>
      <td style="text-align:center">1</td>
      <td style="text-align:left">1 for log files</td>
    </tr>
  </tbody>
</table>### BigConnect

<table>
  <thead>
    <tr>
      <th style="text-align:left">Component</th>
      <th style="text-align:left">Minimum Memory</th>
      <th style="text-align:center">Min CPU Cores</th>
      <th style="text-align:left">Disks</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><b>Explorer</b>
      </td>
      <td style="text-align:left">
        <p>4 GB</p>
        <p>Recommended: 16GB</p>
      </td>
      <td style="text-align:center">4</td>
      <td style="text-align:left">1 for log files</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Bolt Server</b>
      </td>
      <td style="text-align:left">
        <p>4 GB</p>
        <p>Recommended: 16GB</p>
      </td>
      <td style="text-align:center">4</td>
      <td style="text-align:left">1 for log files</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Data Worker</b>
      </td>
      <td style="text-align:left">2GB</td>
      <td style="text-align:center">2</td>
      <td style="text-align:left">n/a</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Job Worker</b>
      </td>
      <td style="text-align:left">2GB</td>
      <td style="text-align:center">2</td>
      <td style="text-align:left">n/a</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Discovery</b>
      </td>
      <td style="text-align:left">
        <p>4GB</p>
        <p>Recommended: 16GB</p>
      </td>
      <td style="text-align:center">4</td>
      <td style="text-align:left">1</td>
    </tr>
  </tbody>
</table>### Druid

| Component | Minimum Memory | Min CPU Cores | Disks |
| :--- | :--- | :---: | :--- |
| **Historical** | Heap size: 0.5GB \* number of CPU cores  No more than 24GB | 4 | 1 |
| **Broker** | Minimum: 4GB  Recommended: 8GB for 15 servers or less 30GB-60GB for 100 servers | 4 | 1 |
| **MiddleManager** | Recommended: 128MB | 2 | 1 SSD Disk |
| **Coordinator** | Same heap size as Broker | 2 | 1 |
| **Overlord** | 25-50% of the Coordinator heap | 2 | 1 |
| **Router** | 512MB | 2 | 1 |

### ElasticSearch

<table>
  <thead>
    <tr>
      <th style="text-align:left">Component</th>
      <th style="text-align:left">Minimum Memory</th>
      <th style="text-align:center">Min CPU Cores</th>
      <th style="text-align:left">Disks</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><b>Server</b>
      </td>
      <td style="text-align:left">
        <p>Minimum: 4GB
          <br />Recommended: 64GB</p>
        <p>No more than 24GB Heap</p>
      </td>
      <td style="text-align:center">4</td>
      <td style="text-align:left">1 for log files</td>
    </tr>
  </tbody>
</table>### Hadoop

#### HDFS

<table>
  <thead>
    <tr>
      <th style="text-align:left">Component</th>
      <th style="text-align:left">Minimum Memory</th>
      <th style="text-align:center">Min CPU Cores</th>
      <th style="text-align:left">Disks</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><b>JournalNode</b>
      </td>
      <td style="text-align:left">1 GB</td>
      <td style="text-align:center">1</td>
      <td style="text-align:left">1 dedicated</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>NameNode</b>
      </td>
      <td style="text-align:left">
        <p>1 GB
          <br />(for POC deployments)</p>
        <p></p>
        <p>Add 1 GB for each additional 1,000,000 blocks</p>
      </td>
      <td style="text-align:center">4 dedicated</td>
      <td style="text-align:left">
        <p>Min 2 dedicated
          <br />for metadata</p>
        <p></p>
        <p>1 dedicated for log files
          <br />(can be shared with the OS)</p>
        <p></p>
        <p>Max: 4</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Data Node</b>
      </td>
      <td style="text-align:left">
        <p>4GB</p>
        <p>Increase the memory for higher replica counts or a higher number of blocks
          per DataNode</p>
      </td>
      <td style="text-align:center">4</td>
      <td style="text-align:left">
        <p>Min: 4</p>
        <p>Max: 24</p>
      </td>
    </tr>
  </tbody>
</table>{% hint style="danger" %}
Running BigConnect on storage platforms other than direct-attached physical disks can provide suboptimal performance. BigConnect and most of the Hadoop platform are optimized to provide high performance by distributing work across a cluster that can utilize data locality and fast local I/O.
{% endhint %}

#### YARN

<table>
  <thead>
    <tr>
      <th style="text-align:left">Component</th>
      <th style="text-align:left">Minimum Memory</th>
      <th style="text-align:center">Min CPU Cores</th>
      <th style="text-align:left">Disks</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><b>Job History Server</b>
      </td>
      <td style="text-align:left">
        <p>1 GB</p>
        <p>Increase memory by 1.6 GB for each 100,000 tasks kept in memory</p>
      </td>
      <td style="text-align:center">1</td>
      <td style="text-align:left">n/a</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>NodeManager</b>
      </td>
      <td style="text-align:left">1 GB</td>
      <td style="text-align:center">
        <p>Min</p>
        <p>8-16
          <br />
          <br />Recommended 32-64</p>
      </td>
      <td style="text-align:left">
        <p>Min: 8</p>
        <p></p>
        <p>Recommended: 12 or more</p>
        <p></p>
        <p><b>Networking:</b>
          <br />Min: dual 1Gbps or faster
          <br />Recommended: single/Dual 10 Gbps or faster</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>ResourceManager</b>
      </td>
      <td style="text-align:left">1GB</td>
      <td style="text-align:center">1</td>
      <td style="text-align:left">1 for log files</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Other Settings</b>
      </td>
      <td style="text-align:left">
        <p>Set the ApplicationMaster Memory YARN configuration property to 512 MB</p>
        <p>Set the Container Memory Minimum YARN configuration property to 1 GB.</p>
      </td>
      <td style="text-align:center">n/a</td>
      <td style="text-align:left">n/a</td>
    </tr>
  </tbody>
</table>### Hive

<table>
  <thead>
    <tr>
      <th style="text-align:left">Component</th>
      <th style="text-align:left">Minimum Memory</th>
      <th style="text-align:center">Min CPU Cores</th>
      <th style="text-align:left">Disks</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">HiveServer2</td>
      <td style="text-align:left">
        <p>Single Connection: 4GB
          <br />2-10 connections: 4-6GB
          <br />11-20 connections: 6- 12GB
          <br />21-40 connections: 12-16 GB
          <br />41 to 80 connections: 16- 24 GB</p>
        <p></p>
        <p>Split HiveServer2 into multiple instances and load balance them once you
          start allocating more than 12 GB to HiveServer2</p>
      </td>
      <td style="text-align:center">4 dedicated</td>
      <td style="text-align:left">1 for log files</td>
    </tr>
    <tr>
      <td style="text-align:left">Hive Metastore</td>
      <td style="text-align:left">Single Connection: 4GB
        <br />2-10 connections: 4-6GB
        <br />11-20 connections: 6- 12GB
        <br />21-40 connections: 12-16 GB
        <br />41 to 80 connections: 16- 24 GB</td>
      <td style="text-align:center">4 dedicated</td>
      <td style="text-align:left">1 for log files</td>
    </tr>
    <tr>
      <td style="text-align:left">Beeline CLI</td>
      <td style="text-align:left">2GB</td>
      <td style="text-align:center">n/a</td>
      <td style="text-align:left">n/a</td>
    </tr>
    <tr>
      <td style="text-align:left">Hive-on-Spark</td>
      <td style="text-align:left">Minimum: 16 GB
        <br />Recommended: 32 GB for larger data sizes
        <br />
        <br />Individual executor heaps should be no larger than 16 GB so machines with
        more RAM can use multiple executors.</td>
      <td style="text-align:center">Min: 4 cores Recommended: 8 cores for larger data sizes</td>
      <td style="text-align:left">Disk space requirements are driven by scratch space requirements for Spark
        spill.</td>
    </tr>
  </tbody>
</table>### Kafka

Kafka requires a fairly small amount of resources, especially with some configuration tuning. By default, Kafka, can run on as little as 1 core and 1GB memory with storage scaled based on requirements for data retention.

CPU is rarely a bottleneck because Kafka is I/O heavy, but a moderately-sized CPU with enough threads is still important to handle concurrent connections and background tasks.

Kafka brokers tend to have a similar hardware profile to HDFS data nodes. How you build them depends on what is important for your Kafka use cases. Use the following guidelines:

| Component | Memory | CPU Cores | Disks |
| :--- | :--- | :---: | :--- |
| **Broker** | 64 GB  Recommended Java heap: 4 GB | 12- 24 cores | 1 HDD For operating system 1 HDD for Zookeeper dataLogDir 10 HDDs, using Raid 10, for Kafka data |
| **MirrorMaker** | 1 GB heap | 1 core per 3-4 streams | n/a |

### Presto

<table>
  <thead>
    <tr>
      <th style="text-align:left">Component</th>
      <th style="text-align:left">Memory</th>
      <th style="text-align:center">Min CPU Cores</th>
      <th style="text-align:left">Disks</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><b>Worker</b>
      </td>
      <td style="text-align:left">
        <p>4GB</p>
        <p>Recommended: 16GB</p>
      </td>
      <td style="text-align:center">4</td>
      <td style="text-align:left">1 SSD Disk</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Coordinator</b>
      </td>
      <td style="text-align:left">
        <p>4GB</p>
        <p>Recommended: 16GB</p>
      </td>
      <td style="text-align:center">4</td>
      <td style="text-align:left">1</td>
    </tr>
  </tbody>
</table>### Spark

| Component | Minimum Memory | Min CPU Cores | Disks |
| :--- | :--- | :---: | :--- |
| **History Server** | 512MB | 1 | 1 for log files |

{% hint style="danger" %}
Spark can be very memory and CPU intensive. We recommend that you adjust the number of CPUs and memory for the Spark History Server based on your specific cluster usage patterns.
{% endhint %}



