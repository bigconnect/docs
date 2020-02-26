# Viewing Jobs

Any command you enter in H2O \(such as `importFiles`\) is submitted as a job, which is associated with a key. The key identifies the job within H2O and is used as a reference.

## Viewing All Jobs

To view all jobs, click the **Admin** menu, then click **Jobs**, or enter `getJobs` in a cell in CS mode.

![](../../../../.gitbook/assets/image%20%28296%29.png)

The following information displays:

* Type \(for example, `Frame` or `Model`\)
* Link to the object
* Description of the job type \(for example, `Parse` or `GBM`\)
* Start time
* End time
* Run time

To refresh this information, click the **Refresh** button. To view the details of the job, click the **View** button.

## Viewing Specific Jobs

To view a specific job, click the link in the “Destination” column.

![](../../../../.gitbook/assets/image%20%28144%29.png)

The following information displays:

* Type \(for example, `Frame`\)
* Link to object \(key\)
* Description \(for example, `Parse`\)
* Status
* Run time
* Progress

Ok, now that you understand how to find jobs in H2O, let’s submit a new one by building a model.

