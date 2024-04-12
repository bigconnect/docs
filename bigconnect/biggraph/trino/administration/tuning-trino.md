# Tuning Trino

The default Trino settings should work well for most workloads. The following information may help you, if your cluster is facing a specific performance problem.

### Config properties[#](https://trino.io/docs/current/admin/tuning.html#config-properties) <a href="#config-properties" id="config-properties"></a>

See Properties reference.

### JVM settings[#](https://trino.io/docs/current/admin/tuning.html#jvm-settings) <a href="#jvm-settings" id="jvm-settings"></a>

The following can be helpful for diagnosing garbage collection (GC) issues:

```
-Xlog:gc*,safepoint::time,level,tags,tid
```
