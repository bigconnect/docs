# Using Workflow Metrics

_Workflow Metrics_ gives insight into the time spent to complete certain workflow events. To use it, set up deadlines on a workflow process’s events. These deadline configurations are referred to as SLAs (Service Level Agreements). Once defined, Workflow Reports measure compliance with the SLAs. They’re like a contract between the workflow participants and Users submitting workflow items. _Workflow Reports_ shows data for all processes with SLAs, including each workflow item’s SLA status: on time or overdue.

IMPORTANT

* _Editing a Workflow with SLAs:_ Editing a workflow (e.g., removing nodes, editing a task name) with SLAs defined on it may invalidate the SLA for items already in the workflow/SLA pipeline.
* _Creating or Editing SLAs for Active Processes:_ Editing an SLA’s duration or defining a new SLA while items are already in the workflow process causes a recalculation for all instances currently in the workflow. Completed workflow instances are not recalculated.

### Prerequisites <a href="#prerequisites" id="prerequisites"></a>

To use _Workflow Metrics_, you must be using Elasticsearch to index your DXP data. To learn more, read the Installing Elasticsearch article.

### Adding SLAs <a href="#adding-slas" id="adding-slas"></a>

To add a SLA,

1. In the Global Menu, navigate to the _Applications_ → _Workflow_ → _Metrics_.
2. Click on the title of the Process.
3.  If there’s no SLA for the process, a warning message stating as much appears. Click the _Add a new SLA_ link from the warning to access the New SLA form directly.

    Alternatively, click the Options (![Options](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-options.png)) menu and select _SLA Settings_.

    ![Add SLAs to a workflow definition from the Metrics application.](https://resources.learn.liferay.com/images/dxp/latest/en/process-automation/workflow/using-workflows/using-workflow-metrics/images/01.png)
4. On the SLAs screen, click the _Add_ button (![Add](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png)).
5. In the New SLA form, give the SLA a Name and Description.
6. Define the time frame for the SLA, specifying three things:
   * **Start**: Enters Task: Review
   * **Pause**: On Task: Update
   * **Stop**: Process Ends: Approved
7.  Define the duration (i.e., the deadline) for the SLA. Fill out at least one of the two time boxes.

    * **Days:** 1
    * **Hours:** 00:00

    ![SLA example](https://resources.learn.liferay.com/images/dxp/latest/en/process-automation/workflow/using-workflows/using-workflow-metrics/images/03.png)
8. Click _Save_.

![Manage SLAs from the SLAs screen.](https://resources.learn.liferay.com/images/dxp/latest/en/process-automation/workflow/using-workflows/using-workflow-metrics/images/02.png)

#### Valid Start and Stop Events <a href="#valid-start-and-stop-events" id="valid-start-and-stop-events"></a>

Any workflow task can be used as a start or end parameter for the SLA.

When the item makes it to the event defined here, the SLA timer begins counting. Choose between these options:

| Start Event       | Description                                                                                                                           |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| The Start Node    | The SLA timer starts when entering the _created_ node.                                                                                |
| Entry into a Task | The SLA clock begins when the workflow transitions to the task.                                                                       |
| Exit from a Task  | The SLA clock begins when the workflow transitions out of the task. For example, the SLA timer starts when leaving the _Review_ Task. |

If the item makes it to the Stop event before the defined SLA duration (the deadline), it’s _On Time_ according to the SLA. If it fails to make it to the Stop event in the specified duration, it’s _Overdue_. When defining the tasks to act as the SLA’s Stop Events, choose between these options:

| Stop Event        | Description                                                                                       |
| ----------------- | ------------------------------------------------------------------------------------------------- |
| Entry into a Task | The SLA clock stops when the workflow transitions to the task (for example, an _Update_ Task).    |
| Exit from a Task  | The SLA clock stops when the workflow transitions out of the task (for example, a _Review_ Task). |
| The end node      | The SLA timer stops when entering the _approved_ node.                                            |

The Pause field defines an event in the workflow when time should stop counting. For the Single Approver workflow, you might choose to pause the SLA timer when the item is in the Update task. The SLA can be paused at any task that falls between the start node and the end node, and it is defined by setting the node(s) when the SLA should be paused. _The SLA timer is paused the entire time a workflow item is in the specified node_.

#### Durations <a href="#durations" id="durations"></a>

| Unit of Time | Instruction                                                                                                                                                              |
| ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Days         | Enter a whole number for number of days. If there are partial days such as 36 hours, use in combination with the _Hours_ field; express it 1 Day and 12 Hours.           |
| Hours        | Enter the number of hours and minutes. The values in the _Hours_ box must not exceed _23:59_; if the duration exceeds one day, use in combination with the _Days_ field. |

### Configuring the SLA Job Interval <a href="#configuring-the-sla-job-interval" id="configuring-the-sla-job-interval"></a>

The time interval between metrics recalculation is called the _SLA Job Interval_. The default value is 1 minute. To change it,

1. From the Applications Menu (![Applications Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)), go to the System Settings application and open the Workflow Metrics entry.
2. Change the Check SLA Job Interval configuration value and click _Save_.

Alternatively,

1.  Create a configuration file called

    ```log
    com.liferay.portal.workflow.metrics.internal.configuration.WorkflowMetricsConfiguration.config
    ```
2.  Give it contents like these:

    ```properties
    checkSLAJobInterval="1"
    ```
3. Place it in `[Home]/osgi/configs`.

Workflow Metrics data is read from the search engine to calculate metrics. When configuring the interval value, consider these competing performance concerns:

* A long time between metrics recalculation events corresponds with more search documents being read during each recalculation.
* A short time between metrics recalculation events corresponds with a higher frequency of read requests to the search engine.

### Re-Indexing Workflow Metrics <a href="#re-indexing-workflow-metrics" id="re-indexing-workflow-metrics"></a>

A _Re-Index_ action in the portal completely deletes, then recreates, search indexes based on mapping files. Because metrics are also stored in the database, there’s no danger of data loss when re-indexing. To Re-index Workflow Metrics on version 7.3,

1. From the Metrics application, open the Options menu (![Options](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-options.png)) and click _Settings_.
2.  Now in the Workflow Index Actions screen, click _Reindex All_ for the Workflow Indexes entry.

    This option acts on every index in the Workflow Metrics application. More granular options are available as well. If you have multiple Virtual Instances, you must reindex each one’s Workflow Metrics separately.

On version 7.2, Workflow Metrics are re-indexed from the Search administrative panel (Control Panel → Configuration → Search).

A re-index of Workflow Metrics is required when the search engine is first configured and each time it’s upgraded. A good rule of thumb is that Workflow Metrics should be re-indexed each time the portal’s main search indexes are re-indexed.

From the overall metrics of a workflow process down to the details on a single item in the workflow, the new Workflow Metrics functionality gives you insights into the time it takes to _get things done_ in the portal.

### Managing Workflow Tasks in Bulk <a href="#managing-workflow-tasks-in-bulk" id="managing-workflow-tasks-in-bulk"></a>

Within the metrics UI you can manage workflow tasks in bulk. For example, you can assign multiple tasks to a single user without going into each task separately. To explore this functionality make sure you have some items that need managing inside a workflow process. For example, [activate](https://learn.liferay.com/web/guest/w/dxp/process-automation/workflow/using-workflows/activating-workflow) the Single Approver workflow process for blogs, then submit a few blogs to the workflow.

1. From the Metrics application, click the workflow process (e.g., Single Approver) to open its dashboard view.
2. Hover over the _Total Pending_ box and click _See Items_ to open the table of pending workflow items with the Pending status.
3.  Select the items you want to reassign in the workflow and click the reassign button (![Reassign](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-move.png)).

    ![Reassign multiple workflow tasks at once.](https://resources.learn.liferay.com/images/dxp/latest/en/process-automation/workflow/using-workflows/using-workflow-metrics/images/04.png)
4. In the Select Tasks to Reassign modal, select the tasks again and click _Next_.
5.  To assign all the tasks to the same assignee, check the box _Use the same assignee for all tasks._, then search for the assignee.

    To select assignees manually for each task, use the New Assignee box for each item to select the assignee.

    ![Select the assignee for each workflow task, or use the same assignee for all tasks.](https://resources.learn.liferay.com/images/dxp/latest/en/process-automation/workflow/using-workflows/using-workflow-metrics/images/05.png)
6. Click Reassign.

Following a similar set of steps, you can also update the due date (![Calendar](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-calendar.png)) of selected workflow items or transition (![Transition](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-transition-arrow.png)) them to another step in the workflow.

![Transition multiple workflow items at once to a new step in the workflow.](https://resources.learn.liferay.com/images/dxp/latest/en/process-automation/workflow/using-workflows/using-workflow-metrics/images/06.png)
