# Hadoop

This is the "production-grade" storage type. It's currently being used by large customers to store billions of vertices and edges. It uses [Apache Accumulo](https://accumulo.apache.org/) as the underlying NoSQL data store and is designed to run in clusters of hundreds of nodes.

BigConnect Core includes a distribution of [HDFS](https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html) that was specifically tuned to work in a containerized deployment environment (Docker Swarm and Kubernetes).
