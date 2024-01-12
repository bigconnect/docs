# Troubleshooting an Upgrade

Use the following tips for help with upgrades:

**After upgrading a BigConnect Pipelines that is registered with BigConnect Pipelines Control Hub, the BigConnect Pipelines fails to start with the following error about a component ID**:

```
Failed registration for component ID '<unique ID>'
```

The upgraded BigConnect Pipelines is using a new component ID stored in the `$SDC_DATA/sdc.id` file. An upgraded BigConnect Pipelines that is registered with Control Hub must use the same component ID as the previous version.\
\
Verify that the `$SDC_DATA` directory that the upgraded BigConnect Pipelines is using includes the previous `sdc.id` file. Then, restart BigConnect Pipelines.\
\
**After upgrading a BigConnect Pipelines that is registered with Control Hub, the BigConnect Pipelines fails to start with the following error about the authentication token:**

```
appAuthToken cannot be NULL or empty
```

The upgraded BigConnect Pipelines is using the new version of the `$SDC_CONF/application-token.txt` file. The previous version of the file includes the authentication token that this BigConnect Pipelines instance requires to issue authenticated requests to Control Hub. As a result, an upgraded BigConnect Pipelines must use the previous version of the file.\
\
Verify that the `$SDC_CONF` directory that the upgraded BigConnect Pipelines is using includes the previous `application-token.txt` file. Then, restart BigConnect Pipelines.
