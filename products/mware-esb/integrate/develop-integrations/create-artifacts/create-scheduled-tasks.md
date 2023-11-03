# Create Scheduled Tasks

Follow the instructions given below to create a **Scheduled Task** in WSO2 Integration Studio.

### Instructions[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/creating-artifacts/creating-scheduled-task/#instructions) <a href="#instructions" id="instructions"></a>

#### Creating the Scheduled Task artifact[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/creating-artifacts/creating-scheduled-task/#creating-the-scheduled-task-artifact) <a href="#creating-the-scheduled-task-artifact" id="creating-the-scheduled-task-artifact"></a>

1.  Right-click the [ESB Config project](https://apim.docs.wso2.com/en/4.2.0/integrate/develop/create-integration-project/#esb-config-project) and click **New** → **Scheduled Task**.

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/scheduled\_task/select-scheduled-task.png)
2.  Select **Create a New Scheduled Task Artifact** and click **Next**.

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/scheduled\_task/new-task-wizard-1.png)
3.  Specify values for the [required parameter](https://apim.docs.wso2.com/en/4.2.0/reference/synapse-properties/scheduled-task-properties) for the scheduled task.

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/scheduled\_task/new-task-wizard-2.png)
4. Specify the location to save the artifact:
   * To save the scheduled task in an existing ESB Config project in your workspace, click **Browse** and select that project.
   * To save the scheduled task in a new ESB Config project, click **Create new Project** and create the new project.
5.  Click **Finish**.&#x20;

    The scheduled task is created in the `src/main/synapse-config/tasks` folder under the ESB Config project you specified.
6. To use the scheduled task, [update the properties](https://apim.docs.wso2.com/en/latest/integrate/develop/creating-artifacts/creating-scheduled-task/#updating-properties).

#### Updating properties[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/creating-artifacts/creating-scheduled-task/#updating-properties) <a href="#updating-properties" id="updating-properties"></a>

Update the task properties to specify the incoming message that should trigger the task and the destination to which the message should be injected.

1.  Open the new artifact from the project explorer.

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/scheduled\_task/scheduled-task-form-view.png)
2. In the **Form** view, you can optionally modify already specified property values.
3.  Click **Task Implementation Properties** to open the **Task Properties** dialog box.

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/scheduled\_task/scheduled-task-properties.png)
4. Update the properties.

\
