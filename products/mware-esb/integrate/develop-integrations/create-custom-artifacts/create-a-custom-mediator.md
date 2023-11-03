# Create a Custom Mediator

If you need to create a custom mediator that performs some logic on a message, you can either create a new mediator project, or import an existing mediator project using WSO2 Integration Studio.

Once a mediator project is finalized, you can export it as a deployable artifact by right-clicking on the project and selecting **Export Project as Deployable Archive** . This creates a JAR file that you can deploy. Alternatively, you can group the mediator project as a Composite Application Project, create a Composite Application Archive (CAR), and deploy it to the Micro Integrator.

Info

A URL classloader is used to load classes in the mediator (class mediators are not deployed as OSGi bundles). Therefore, it is only possible to refer to the class mediator from artifacts packed in the same CAR file in which the class mediator is packed. Accessing the class mediator from an artifact packed in another CAR file is not possible. However, it is possible to refer to the class mediator from a sequence packed in the same CAR file and call that sequence from any other artifact packed in other CAR files.

### Instructions[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/customizations/creating-custom-mediators/#instructions) <a href="#instructions" id="instructions"></a>

#### Creating a Mediator Project[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/customizations/creating-custom-mediators/#creating-a-mediator-project) <a href="#creating-a-mediator-project" id="creating-a-mediator-project"></a>

Create this project directory to start creating custom mediator artifacts. You can use these customer mediators when you define the mediation flow in your ESB config project.

1. Open WSO2 Integration Studio and click Miscellaneous → Create Mediator Project in the Getting Started view as shown below. ![new mediator project](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_project/new\_mediator\_project.png)
2. In the dialog that opens, select Create New Mediator and click Next.
3. Enter a project name, package name, and class name. ![new mediator dialog](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_project/new\_mediator\_artifact\_dialog.png)
4. Click Finish and see that the project is now listed in the project explorer.

The mediator project is created in the workspace location you specified with a new mediator class that extends `org.apache.synapse.mediators.AbstractMediator`.

#### Importing a Java Mediator Project[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/customizations/creating-custom-mediators/#importing-a-java-mediator-project) <a href="#importing-a-java-mediator-project" id="importing-a-java-mediator-project"></a>

Follow the steps below to import a Java mediator project (that includes a Java class, which extends the `org.apache.synapse.mediators.AbstractMediator` class) to WSO2 Integration Studio.

1. Open WSO2 Integration Studio and click on Create Mediator Project in the Getting Started view as shown above.
2. In the dialog that opens, select **Import From Workspace** and click **Next**.
3. Specify the mediator project in this workspace that you want to import. Only projects with source files that extend `org.apache.synapse.mediators.AbstractMediator` are listed. Optionally, you can change the location where the mediator project will be created and add it to working sets.
4. Click **Finish**.

The mediator project you selected is created in the location you specified.
