# Sponge Annotator

The Sponge Annotator is a Google Chrome extension that helps users mark sections of web pages to be extracted. 

## Accessing Sponge Annotator

Once the Google Chrome extension is installed, it needs to be configured so it can talk to Sponge Manager. Please refer to the [Configure Sponge Annotator](sponge-annotator.md#configure-sponge-annotator) section below.

Once the extension is configured, it can be accessed by opening **Developer Tools \(F12\)**. You will see a tab named **Sponge Annotator**.

![](../../../.gitbook/assets/image%20%28549%29.png)

There are three menu options in the Sponge Annotator toolbar:

* **Projects** - shows the list of projects. 
* **Project** - actions available for the selected project
* **Create new...**  - create a new project

The list of projects is loaded automatically from Sponge Manager if the Sponge Manager connection is made successfully.

### Annotate a web page

Choose a project from the list of projects to get started. You will be taken to the project details panel.

![](../../../.gitbook/assets/image%20%2873%29.png)

The first thing you need to do is to **Add a new field.** This will take you to the add new field panel where you can perform several actions.

![](../../../.gitbook/assets/image%20%2862%29.png)

* **Field**: the name of the field \(no spaces allowed\)
* **Selector**: 
  * **Select** - click select and choose an area of the page you would like to extract. This will result in generating a CSS selector for the element you selected.
  * **Element preview** - after choosing and validating the CSS selector, click this button to see the selected element on the page
  * **Data preview**: after choosing and validating the CSS selector, click this button to preview what text would be extracted from the page using the CSS selector. 
* **Regex**: a regex expression to apply on the extracted value

Once you are happy with your selection, click **Save field** to continue. The field will be added to the project details panel.

![](../../../.gitbook/assets/image%20%28429%29.png)

You have some actions available for the new field: 

* **Element preview** - click this button to see the element on the page
* **Data preview** -  click this button to preview what text would be extracted from the page using the field's CSS selector
* **Edit** - edit field details
* **Delete** - delete the field

{% hint style="success" %}
All changes are automatically synchronized to Sponge Manager when an action is taken in the extension.
{% endhint %}

### Select an area from the web page

Once you clicked the Select button in the field details panel, an overlay will be displayed at the bottom of the webpage and zone selection becomes active.

![](../../../.gitbook/assets/image%20%28452%29.png)

Moving the mouse around the page will show green boxes around the HTML element below the mouse pointer. Click the mouse to select the desired element and the green box will become red. The CSS selector will be updated in the overlay panel with the selected element. 

Once you are finished selecting the desired element, click the **Done selecting!** button to have the CSS selector registered for the field.

![](../../../.gitbook/assets/screenshot-from-2019-12-05-14-01-23.jpg)

The overlay panel has some helper options available:

* **S** - Press the S key to select the HTML element below the mouse pointer. \(same as mouse click\)
* **P** - Press the P key to select the parent of the HTML element below the mouse pointer
* **C** - Press the C key to select the first child of the HTML element below the mouse pointer

## Configure Sponge Annotator

Once the extension is loaded, it will be displayed also as an icon next to Google Chrome's address bar: ![](../../../.gitbook/assets/image%20%28261%29.png) 

Right-click on the icon and choose the **Options** menu item:

![](../../../.gitbook/assets/image%20%28344%29.png)

The configuration page will be displayed in the browser. Here you can configure Sponge Manager connection details:

![](../../../.gitbook/assets/image%20%28266%29.png)

* **Sponge Server URL**: The URL of Sponge Manager. Please check to see if it's accessible from your computer.
* **Username**: your Sponge Manager username
* **Password**: your Sponge Manager user password



