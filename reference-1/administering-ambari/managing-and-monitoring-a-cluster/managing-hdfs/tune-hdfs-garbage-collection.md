# Tune HDFS garbage collection

**About this task**  
The Concurrent Mark Sweep \(CMS\) garbage collection \(GC\) process includes a set of heuristic rules used to trigger garbage collection. This makes garbage collection less predictable and tends to delay collection until capacity is reached, creating`a Full GC error` \(which might pause all processes\). Ambari sets default parameter values for many properties during cluster deployment. Within the export HADOOP\_NameNode\_Opts= clause of the hadoop-env template, two parameters that affect the CMS GC process have the following default settings:

* `-XX:+UseCMSInitiatingOccupancyOnly`

  prevents the use of GC heuristics.

* -XX:CMSInitiatingOccupancyFraction=&lt;percent&gt;

  tells the Java VM when the CMS collector should be triggered.

If this percent is set too low, the CMS collector runs too often; if it is set too high, the CMS collector is triggered too late, and [concurrent mode failure](http://www.oracle.com/technetwork/java/javase/gc-tuning-6-140523.html#cms.concurrent_mode_failure) might occur. The default setting for `-XX:CMSInitiatingOccupancyFraction` is 70, which means that the application should utilize less than 70% of capacity. To tune garbage collection by modifying the NameNode CMS GC parameters, follow these steps:

**Steps**

1. In **Ambari Web**, browse to **Services &gt; HDFS**. 
2. Open the **Configs** tab and browse to **Advanced &gt; Advanced hadoop-env**. 
3. Edit the hadoop-env template. 
4. Save your configurations and restart, as prompted.

  


