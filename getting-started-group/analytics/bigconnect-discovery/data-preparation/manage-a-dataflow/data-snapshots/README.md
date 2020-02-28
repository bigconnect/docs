# Data snapshots

When rule editing is complete, you can create a data snapshot of the finalized dataset, which can then be downloaded to your local PC or ingested into the Druid engine. Running the data snapshot applies the rules to the entire data, which, in the process of rule editing, applied to a sample dataset of less than 10,000 rows.

Below are instructions on creating a snapshot:

1 Click the **Data Snapshot** button on the upper right of the [Edit rules](../dataflow-rules.md) window.

![](../../../../../../.gitbook/assets/image%20%28194%29.png)

2. When a popup is displayed to set snapshot options, select either FileSystem or HIVE \(STAGING\_DB\) under Snapshot type.

![](../../../../../../.gitbook/assets/image%20%28438%29.png)

If FileSystem is selected as the snapshot location, the snapshot will be created as **CSV** or **JSON**.

![](../../../../../../.gitbook/assets/image%20%28442%29.png)

The HIVE option is available only when STAGING\_DB is enabled. A snapshot is created in the table when you designate a schema name and table name.

![](../../../../../../.gitbook/assets/image%20%28609%29.png)

3. When the snapshot is created, you can view the snapshot status and related information in the same window.

![](../../../../../../.gitbook/assets/image%20%28595%29.png)





