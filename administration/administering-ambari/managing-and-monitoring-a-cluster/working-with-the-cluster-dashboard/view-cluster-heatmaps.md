# View cluster heatmaps

The Ambari Web **Heatmaps** page provides a graphical representation of your overall cluster utilization, using simple color coding known as a heatmap.

A colored block represents each host in your cluster. Colors displayed in the block represent usage in a unit appropriate for the selected set of metrics.

Hovering over a block that represents a host displays more detailed information about that specific host. A separate window displays metrics about components installed on that host.

![](../../../../.gitbook/assets/image%20%28118%29.png)

If any data necessary to determine usage is not available, the block displays **Invalid data**.

You can select a different heatmap, using the **Select Metric** menu:

![](../../../../.gitbook/assets/image%20%28152%29.png)

Heatmaps supports the following metrics:

**Host/Disk Space Used %**  
disk.disk\_free and disk.disk\_total  
  
**Host/Memory Used %**  
memory.mem\_free and memory.mem\_total  
  
**Host/CPU Wait I/O %**  
cpu.cpu\_wio  
  
**HDFS/Bytes Read**  
dfs.datanode.bytes\_read  
  
**HDFS/Bytes Written**  
dfs.datanode.bytes\_written  
  
**HDFS/Garbage Collection Time**  
jvm.gcTimeMillis  
  
**HDFS/JVM Heap MemoryUsed**  
jvm.memHeapUsedM  
  
**YARN/Garbage Collection Time**  
jvm.gcTimeMillis  
  
**YARN / JVM Heap Memory Used**  
jvm.memHeapUsedM  
  
**YARN / Memory used % Used**  
MemoryMB and AvailableMemoryMB  
  
**HBase/RegionServer read request count**  
hbase.regionserver.readRequestsCount  
  
**HBase/RegionServer write request count**  
hbase.regionserver.writeRequestsCount  
  
**HBase/RegionServer compaction queue size**  
hbase.regionserver.compactionQueueSize  
  
**HBase/RegionServer regions**  
hbase.regionserver.regions  
  
**HBase/RegionServer**  
memstore sizes hbase.regionserver.memstoreSizeMB

