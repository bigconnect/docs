# Create a Message Processor

Follow the instructions given below to create a new [Message Processor](https://apim.docs.wso2.com/en/4.2.0/reference/synapse-properties/about-message-stores-processors) artifact in WSO2 Integration Studio.

### Instructions[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/creating-artifacts/creating-a-message-processor/#instructions) <a href="#instructions" id="instructions"></a>

#### Creating the Message Processor artifact[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/creating-artifacts/creating-a-message-processor/#creating-the-message-processor-artifact) <a href="#creating-the-message-processor-artifact" id="creating-the-message-processor-artifact"></a>

1.  Right-click the [ESB Config project](https://apim.docs.wso2.com/en/4.2.0/integrate/develop/create-integration-project/#esb-config-project) and go to **New → Message Processor** to open the **New Message Processor Artifact** dialog box.

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/new\_message\_processor/select-message-processor.jpg)
2.  Select **Create a new message-processor artifact** and click **Next**.

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/new\_message\_processor/new-message-processor-wizard-1.png)
3.  Enter a unique name for this message processor, specify the type of processor you're creating.

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/new\_message\_processor/new-message-processor-wizard-2.png)

    See the links given below for descriptions of properties for each message processor type:

    * [Message Sampling Processor properties](https://apim.docs.wso2.com/en/4.2.0/reference/synapse-properties/message-processors/msg-sampling-processor-properties)
    * [Scheduled Message Forwarding Processor properties](https://apim.docs.wso2.com/en/4.2.0/reference/synapse-properties/message-processors/msg-sched-forwarding-processor-properties)
    * [Scheduled Failover Message Forwarding Processor properties](https://apim.docs.wso2.com/en/4.2.0/reference/synapse-properties/message-processors/msg-sched-failover-forwarding-processor-properties)
4. Do one of the following to save the artifact:
   * To save the message processor in an existing ESB Config project in your workspace, click **Browse** and select that project.
   * To save the message processor in a new ESB Config project, click **Create new Project** and create the new project.
5. Click **Finish**.&#x20;

The message processor is created in the `src/main/synapse-config/message-processors` folder under the ESB Config project you specified.

#### Updating the properties[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/creating-artifacts/creating-a-message-processor/#updating-the-properties) <a href="#updating-the-properties" id="updating-the-properties"></a>

Open the new message processor artifact from the project explorer. You can use the **Form** view or the **Source** view to update message processor properties.

![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/new\_message\_processor/message-processor-form-view.png)

See the links given below for descriptions of properties for each processor type:

* [Message Sampling Processor properties](https://apim.docs.wso2.com/en/4.2.0/reference/synapse-properties/message-processors/msg-sampling-processor-properties)
* [Scheduled Message Forwarding Processor properties](https://apim.docs.wso2.com/en/4.2.0/reference/synapse-properties/message-processors/msg-sched-forwarding-processor-properties)
* [Scheduled Failover Message Forwarding Processor properties](https://apim.docs.wso2.com/en/4.2.0/reference/synapse-properties/message-processors/msg-sched-failover-forwarding-processor-properties)
