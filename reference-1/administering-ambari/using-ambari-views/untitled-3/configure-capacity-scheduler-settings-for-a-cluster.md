# Configure capacity scheduler settings for a cluster



You can use the Scheduler box to set global capacity scheduler settings that apply to all queues.

![](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.3.0/using-ambari-views/how-to/images/amb_views_cs_scheduler_settings.png)

The following Scheduler global parameters are available:

* Maximum Applications – To avoid system-thrash due to an unmanageable load -- caused either by malicious users, or accidentally -- the Capacity Scheduler enables you to place a static, configurable limit on the total number of concurrently active \(both running and pending\) applications at any one time. This property is used to set this limit, with a default value of 10,000.
* Maximum AM Resource – The limit for running applications in any specific queue is a fraction of this total limit, proportional to its capacity. This is a hard limit, which means that once this limit is reached for a queue, any new applications submitted to that queue will be rejected, and clients will have to wait and retry later.
* Node Locality Delay – The number of missed scheduling cycles after which the scheduler attempts to schedule rack-local containers.
* Calculator – The method by which the scheduler calculates resource capacity across resource types.
* Queue Mappings – You can use this box to specify default queue mapping based on user or group.
* Queue Mappings Override – Select this box to enable override of default queue mappings.

