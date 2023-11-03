# Create a Message Store

Follow the instructions given below to create a new [Message Store](https://apim.docs.wso2.com/en/4.2.0/reference/synapse-properties/about-message-stores-processors) artifact in WSO2 Integration Studio.

### Instructions[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/creating-artifacts/creating-a-message-store/#instructions) <a href="#instructions" id="instructions"></a>

#### Creating the Message Store artifact[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/creating-artifacts/creating-a-message-store/#creating-the-message-store-artifact) <a href="#creating-the-message-store-artifact" id="creating-the-message-store-artifact"></a>

1.  Right-click the [ESB Config project](https://apim.docs.wso2.com/en/4.2.0/integrate/develop/create-integration-project/#esb-config-project) and go to **New → Message Store** to open the **New Message Store Artifact** dialog box.

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/new\_message\_store/select-message-store.png)
2.  Select the **Create a new message-store artifact** option and click **Next**.

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/new\_message\_store/new-message-store-wizard-1.png)
3.  Enter a unique name for the message store, and then select the type of message store you are creating.

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/new\_message\_store/new-message-store-wizard-2.png)

    See the links given below for descriptions of message store properties for each store type:

    * [JMS properties](https://apim.docs.wso2.com/en/4.2.0/reference/synapse-properties/message-stores/jms-msg-store-properties)
    * [JDBC properties](https://apim.docs.wso2.com/en/4.2.0/reference/synapse-properties/message-stores/jdbc-msg-store-properties)
    * [RabbitMQ properties](https://apim.docs.wso2.com/en/4.2.0/reference/synapse-properties/message-stores/rabbitmq-msg-store-properties)
    * [Resequence properties](https://apim.docs.wso2.com/en/4.2.0/reference/synapse-properties/message-stores/resequence-msg-store-properties)
    * [WSO2 MB properties](https://apim.docs.wso2.com/en/4.2.0/reference/synapse-properties/message-stores/wso2mb-msg-store-properties)
    * [In-Memory properties](https://apim.docs.wso2.com/en/4.2.0/reference/synapse-properties/message-stores/in-memory-msg-store-properties)
    * [Custom properties](https://apim.docs.wso2.com/en/4.2.0/reference/synapse-properties/message-stores/custom-msg-store-properties)
4. Do one of the following to save the artifact:
   * To save the message store in an existing ESB Config project in your workspace, click **Browse** and select that project.
   * To save the message store in a new ESB Config project, click **Create new Project** and create the new project.
5. Click **Finish**.&#x20;

The message store is created in the `src/main/synapse-config/message-stores` folder under the ESB Config project you specified.

#### Designing the integration[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/creating-artifacts/creating-a-message-store/#designing-the-integration) <a href="#designing-the-integration" id="designing-the-integration"></a>

To add a message store to the integration sequence, use the [Store Mediator](https://apim.docs.wso2.com/en/4.2.0/reference/mediators/store-mediator):

1. Open to the **Design View** of your [mediation sequence](https://apim.docs.wso2.com/en/4.2.0/reference/synapse-properties/sequence-properties).
2.  Drag the [Store Mediator](https://apim.docs.wso2.com/en/4.2.0/reference/mediators/store-mediator) from the **Palette** and drop it to the relevant position in the [mediation sequence](https://apim.docs.wso2.com/en/4.2.0/reference/synapse-properties/sequence-properties):

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/new\_message\_store/message-store-graphical-editor.png)
3.  Double-click the **Store Mediator** to open the **Properties** tab:

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/new\_message\_store/message-store-properties.png)
4. Select your message store artifact from the list in the **Available Message Stores** field as shown above.

The message store is now linked to your integration sequence.

#### Updating the properties[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/creating-artifacts/creating-a-message-store/#updating-the-properties) <a href="#updating-the-properties" id="updating-the-properties"></a>

Open the new message store artifact from the project explorer. You can use the **Form** view or the **Source** view to update message store properties.

![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/new\_message\_store/message-store-form-view.png)
