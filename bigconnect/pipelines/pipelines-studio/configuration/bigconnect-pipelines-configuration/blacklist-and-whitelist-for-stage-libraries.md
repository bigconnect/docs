# Blacklist and Whitelist for Stage Libraries

By default, almost all installed stage libraries are available for use in BigConnect Pipelines. You can use blacklist and whitelist properties to limit the stage libraries that can be used.

To limit the stage libraries created by StreamSets, use one of the following properties:

```
system.stagelibs.whitelist
system.stagelibs.blacklist
```

To limit stage libraries created by other parties, use one of the following properties:

```
user.stagelibs.whitelist
user.stagelibs.blacklist
```

<mark style="color:red;">**Warning:**</mark> Use only the whitelist or blacklist for each set of libraries. Using both can cause BigConnect Pipelines to fail to start.

The MapR stage libraries are blacklisted by default. To use one of the MapR stage libraries, run the MapR setup script as described in MapR Prerequisites.
