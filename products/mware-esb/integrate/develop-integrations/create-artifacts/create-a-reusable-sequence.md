# Create a Reusable Sequence

Follow these steps to create a new, reusable sequence that you can add to your mediation workflow or refer to from a sequence mediator, or to create a sequence mediator and its underlying sequence all at once.

### Instructions[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/creating-artifacts/creating-reusable-sequences/#instructions) <a href="#instructions" id="instructions"></a>

#### Creating a Sequence Artifact[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/creating-artifacts/creating-reusable-sequences/#creating-a-sequence-artifact) <a href="#creating-a-sequence-artifact" id="creating-a-sequence-artifact"></a>

1.  Right-click the [ESB Config project](https://apim.docs.wso2.com/en/4.2.0/integrate/develop/create-integration-project/#esb-config-project) and go to **New → Sequence** to open the **New Sequence Artifact** dialog box.

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/new\_sequence/select-sequence.png)
2.  Select **Create New Sequence** and click **Next**.

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/new\_sequence/new-sequence-wizard-1.png)
3.  Specify a unique name for the sequence.

    Info

    **Creating a Main Sequence**: If you want to create the default main sequence that just sends messages without mediation, be sure to name it `main`, which automatically populates the sequence with the default **In** and **Out** sequences.

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/new\_sequence/new-sequence-wizard-2-1.png)
4. In the **Save Sequence in** field, specify the location to save the sequence:
   * To save the sequence in an existing ESB Config project in your workspace, click **Browse** and select that project. Else, click **Create new Project** and create the new project.
   *   To save the sequence as a **Dynamic Sequence** in a [registry resource project](https://apim.docs.wso2.com/en/4.2.0/integrate/develop/create-integration-project/#registry-resource-project):

       1. Select the **Make this as Dynamic Sequence** check box.
       2. Specify the registry space (Governance or Configuration) in the **Registry** field.
       3. If a **Registry Resource** project already exist in the workspace, click **Browse** for the **Save Sequence in** field and select the registry resource project. Else, click **Create new Project** to create a new registry project.
       4. Type the sequence name in the **Registry Path** field.

       ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/new\_sequence/new-sequence-wizard-2-2.png)
5. Click **Finish**.&#x20;

The sequence is created in the `src/main/synapse-config/sequences` folder under the ESB Config or [registry resource project](https://apim.docs.wso2.com/en/4.2.0/integrate/develop/create-integration-project/#registry-resource-project) you specified.

The sequence is also available in the **Defined Sequences** section of the **Palette** and ready for [use in other meditation workflows](https://apim.docs.wso2.com/en/latest/integrate/develop/creating-artifacts/creating-reusable-sequences/#using-a-sequence).

#### Create from a Sequence Mediator[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/creating-artifacts/creating-reusable-sequences/#create-from-a-sequence-mediator) <a href="#create-from-a-sequence-mediator" id="create-from-a-sequence-mediator"></a>

1.  Open your proxy service, drag the **Sequence Mediator** from the **Palette** to the canvas. This represents a sequence artifact.

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/new\_sequence/add-sequence-mediator.png)
2. If required, change the name of the sequence.
3. Double-click the sequence mediator you just added. The canvas of the new sequence opens in the graphical editor.

The sequence artifact (with the name you specified) is created in the `src/main/synapse-config/sequences` folder under the ESB Config project.

#### Designing the integration[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/creating-artifacts/creating-reusable-sequences/#designing-the-integration) <a href="#designing-the-integration" id="designing-the-integration"></a>

When you create a sequence, it appears in the **Defined Sequences** section of the tool palette. To use this sequence in a mediation flow:

1.  When you sequence artifact from the **Config** project in the project explorer, you will see the default **Design** view.

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/new\_sequence/sequence-design-view.png)
2.  Drag and drop the required integration artifacts from the **Palette** to the canvas and design the integration flow.

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/new\_sequence/sequence-graphical-editor.png)

To use a sequence from a different project or from the registry, you need to use the [Sequence Mediator](https://apim.docs.wso2.com/en/latest/integrate/develop/creating-artifacts/creating-reusable-sequences/):

1. Drag and drop the **Sequence Mediator** from the **Palette** to the mediation flow.
2. Click the **Sequence Mediator** icon to open the **Properties** tab:
3. Click **Static Reference Key**, and then click the browse **\[...]** button on the right.
4. In the **Resource Key Editor**, click **Registry** if the sequence is stored in the registry or **Workspace** if it is in another ESB Config project.
5. If you are trying to select a sequence from the registry and no entries appear in the dialog box, click **Add Registry Connection** and connect to the registry where the sequence reside.
6. Select the required sequence and click **OK**, and then click **OK** again.

The sequence mediator name and static reference key are updated to point to the sequence you selected.

You can also use the [**Source** view](https://apim.docs.wso2.com/en/latest/integrate/develop/creating-artifacts/creating-reusable-sequences/#using-the-source-view) to update the sequence configuration.

#### Using the Source View[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/creating-artifacts/creating-reusable-sequences/#using-the-source-view) <a href="#using-the-source-view" id="using-the-source-view"></a>

Click the **Source** tab to view the XML-based synapse configuration (source code) of the inbound endpoint. You can update the sequence using this view.

![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/new\_sequence/sequence-source-view.png)
