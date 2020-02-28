# View HDFS erasure coding and encryption status

You can view HDFS erasure coding and encryption status in the Ambari Files View. Erasure coding is used to reduce the amount of storage space required for replication. Encryption is a form of data security required in industries such as healthcare.

### Erasure coding

From the Ambari Files view, you can view if an erasure coding policy is set for a specific directory in HDFS. For example, in the following figure, RS-6-3-1024k erasure coding policy is set for the `data` directory.  
  
Figure 1. HDFS erasure coding  
![HDFS erasure coding](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/using-ambari-views/how-to/images/amb_file_view_erasure_coding.png)  
  
The supported erasure coding policies are:  
  
**RS-3-2-1024k**  
Reed-Solomon code with 3 data blocks, 2 parity blocks, and 1024 KB cell size.  
  
**RS-6-3-1024k**  
Reed-Solomon code with 6 data blocks, 3 parity blocks, and 1024 KB cell size.  
  
**RS-LEGACY-6-3-1024k**  
Reed-Solomon legacy code with 6 data blocks, 3 parity blocks, and 1024 KB cell size.  
  
**XOR-2-1-1024k**  
XOR with 2 data blocks, 1 parity block, and 1024 KB cell size.

For more information about erasure coding, see the _Data Storage_ documentation.

### Encryption

From the Ambari Files view, you can view if encryption is enabled for a specific directory in HDFS.

`Yes` is displayed when encryption is enabled for a directory, and `No` is displayed when encryption is disabled. For example, in the following figure, encryption is enabled for the `dir2` directory.  
  
Figure 2. HDFS encryption  
![HDFS encryption](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/using-ambari-views/how-to/images/amb_file_view_encryption.png)

For more information about HDFS encryption, see the _Security_ documentation.

