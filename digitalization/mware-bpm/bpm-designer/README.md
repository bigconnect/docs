# BPM Designer

The MWARE BPM Designer is a powerful process, case, form and decision modeling application. This user guide leads you through the most important aspects of both, the application and the models you can create with it.

This user guide leads you through the most important aspects of both, the application and the models you can create with it.

**How to Log In**

To access the BPM Designer you need to type in the navigation URL into your browser. Once the page is loaded, the login page appears.&#x20;

<figure><img src="../../../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

In order to log in, you need to type in your login and password on the form and click on `Sign in`.

After logging in, your are presented with a list of apps where your can manage various aspects of the product:

<figure><img src="../../../.gitbook/assets/image (2) (1).png" alt=""><figcaption></figcaption></figure>

* **Task App**: Access your full task list and work on any tasks assigned to you from any process app. Also, start new processes and tasks.
* **Modeler App**: Access a central place to model BPMN processes, Cases, DMN decision tables, Form definitions and create app definitions
* **Admin Adpp**: Access the  Admin application.
* **IDM App**: A central place to define users, groups and privileges..

**IDM App**[**​**](https://documentation.flowable.com/latest/user/design/introduction#user-management)

Modeling business applications is often a collaborative effort. For that reason, the BPM Designer has its own built-in user system or can use other user management systems.

To create, modify and delete users in the built-in system, click on your username in the top-right corner of the navigation bar. Select `IDM App` to navigate to the user overview.

<figure><img src="../../../.gitbook/assets/image (3) (1).png" alt=""><figcaption></figcaption></figure>

Click on `Create user` to add a new user to Flowable Design.

<figure><img src="../../../.gitbook/assets/image (891).png" alt=""><figcaption></figcaption></figure>

Enter at least the **username**, **password** and the **first name** to create a new user. All other fields are optional.

You can **edit user details**, **change the password** or **delete a user** if you select a user in the overview and choose the desired actions in the dropdown labeled `Select an action`.

### Model Overview[​](https://documentation.flowable.com/latest/user/design/introduction#model-overview) <a href="#model-overview" id="model-overview"></a>

MWARE BPM applications are largely built on top of so-called "Models". The BPM Designer enables you to edit and create new models and package them up for use in a BPM application (BPM App).

In total, there are seven model types in the BPM Designer: Apps, Processes, Cases, Forms, Pages, Decisions and Other.

You can access your models by clicking on the respective link in the navigation bar. This brings you to an overview of all models of a that type.

<figure><img src="../../../.gitbook/assets/image (4) (1).png" alt=""><figcaption></figcaption></figure>

In this case, we clicked on the **Processes** tab which shows us all existing process models. Since the user interface of all model types is very similar, this provides a good overview of how to work with other model types.

To the left side of the screen, there is a search box which allows you to search through all models.

n the center, you see a preview of all models along with their version, name, creation date and the person who created it. If you click on a model, you are brought to the **Model Detail View**.

On the top-right of the screen, it is possible to create a new model or import an existing one.

### Importing Models[​](https://documentation.flowable.com/latest/user/design/introduction#importing-models) <a href="#importing-models" id="importing-models"></a>

You can import single models from files by pressing the `Import` button on the overview screen of the desired model type. You are presented with the following screen:

<figure><img src="../../../.gitbook/assets/image (892).png" alt=""><figcaption></figcaption></figure>

Depending on the type, the filename must one of the following types:

| Model Type      | File Extension         |
| --------------- | ---------------------- |
| Processes       | _.bpmn or_ .bpmn20.xml |
| Cases           | _.cmmn or_ .cmmn.xml   |
| Forms           | \*.json                |
| Pages           | \*.json                |
| Decision Tables | _.dmn or_ .dmn.xml     |
| Apps            | \*.zip                 |

### Creating New Models[​](https://documentation.flowable.com/latest/user/design/introduction#creating-new-models) <a href="#creating-new-models" id="creating-new-models"></a>

Creating a new one prompts you with the following screen:

<figure><img src="../../../.gitbook/assets/image (893).png" alt=""><figcaption></figcaption></figure>

The **name** of the model is what appears in the overview and should therefore be easy to understand.

The **model key** is a technical identifier which is used in various places to refer to the process on a _technical_ level. Even though you can change the model key later on, you should only ever do that if you are fully aware of the ramifications.

The **description** of a model is for documentation purposes only and can be left empty.

If you click on **Create new model**, you are routed to the model type’s editor, in this case the process editor.

### Model Detail View[​](https://documentation.flowable.com/latest/user/design/introduction#model-detail-view) <a href="#model-detail-view" id="model-detail-view"></a>

If you click on a model in the overview, you are redirected to its Detail View.

There, you see the **name**, **description**, **version**, **creator** and the **modification date** of the model as well as a visual preview of it.

If you want to see an older version of your model, press on the `History` button and choose the version you are interested in.
