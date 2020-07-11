# Adjust Ambari server heap size

Adjust the heap size on the Ambari server host to increase available memory.

1.On the Ambari server host, edit the ambari-env.sh file:

```text
$ vi /opt/bdl/var/lib/ambari-server/ambari-env.sh
```

2. For the `AMBARI_JVM_ARGS` variable, replace the default `-Xmx2048m` with a value such as: `-Xmx4GB -Xmn2GB` based on the number of nodes in your cluster. Use the following recommendations as guidance:

| Number of Cluster Nodes | Xmx value | Xmn value |
| :--- | :--- | :--- |
| 100 - 400 | 4GB | 2GB |
| 400 - 800 | 4GB | 2GB |
| 800 - 1200 | 2GB | 2GB |
| 1200 - 1600 | 16GB | 2.4GB |

