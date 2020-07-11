# Default user accounts

The Ambari Installation wizard creates default service user account names for each installed service.

<table>
  <thead>
    <tr>
      <th style="text-align:left">Service</th>
      <th style="text-align:left">Component</th>
      <th style="text-align:left">Default user account</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">Accumulo</td>
      <td style="text-align:left">Accumulo Tracer
        <br />Accumulo Monitor
        <br />Accumulo GC
        <br />Accumulo Master</td>
      <td style="text-align:left">accumulo</td>
    </tr>
    <tr>
      <td style="text-align:left">Ambari Metrics</td>
      <td style="text-align:left">Metrics Collector
        <br />Metrics Monitor</td>
      <td style="text-align:left">ams</td>
    </tr>
    <tr>
      <td style="text-align:left">HDFS</td>
      <td style="text-align:left">NameNode
        <br />SecondaryNameNode
        <br />DataNode</td>
      <td style="text-align:left">hdfs</td>
    </tr>
    <tr>
      <td style="text-align:left">Hive</td>
      <td style="text-align:left">Hive Metastore
        <br />HiveServer2</td>
      <td style="text-align:left">hive</td>
    </tr>
    <tr>
      <td style="text-align:left">Kafka</td>
      <td style="text-align:left">Kafka Broker</td>
      <td style="text-align:left">kafka</td>
    </tr>
    <tr>
      <td style="text-align:left">Spark</td>
      <td style="text-align:left">Spark History Server</td>
      <td style="text-align:left">spark</td>
    </tr>
    <tr>
      <td style="text-align:left">YARN</td>
      <td style="text-align:left">NodeManager
        <br />ResourceManager</td>
      <td style="text-align:left">yarn</td>
    </tr>
    <tr>
      <td style="text-align:left">ZooKeeper</td>
      <td style="text-align:left">ZooKeeper</td>
      <td style="text-align:left">zookeeper</td>
    </tr>
    <tr>
      <td style="text-align:left">ElasticSearch</td>
      <td style="text-align:left">
        <p>ElasticSearch Master</p>
        <p>ElasticSearch Node</p>
      </td>
      <td style="text-align:left">elasticsearch</td>
    </tr>
    <tr>
      <td style="text-align:left">Druid</td>
      <td style="text-align:left">&lt;all components&gt;</td>
      <td style="text-align:left">druid</td>
    </tr>
    <tr>
      <td style="text-align:left">BigConnect</td>
      <td style="text-align:left">&lt;all components&gt;</td>
      <td style="text-align:left">bigconnect</td>
    </tr>
  </tbody>
</table>

For all components, the Smoke Test user performs smoke tests against cluster services as part of the install process. It also can perform these on-demand, from the Ambari Web UI. The default user account for the smoke test user is ambari-qa.

