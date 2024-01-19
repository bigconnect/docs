# Creating Page Fragments

Fragments enable you to create rich content. With all of the included Fragments and Collections, many projects can be completed using just what ships with MWARE Portal. Sometimes, though, you must create your own Fragments. Fragments are built using HTML, CSS, and JavaScript.

### Creating and Managing Fragments <a href="#creating-and-managing-fragments" id="creating-and-managing-fragments"></a>

To navigate to the Page Fragments interface,

1.  Go to Site Administration. Make sure the Site where you want to work is selected. If you prefer creating a Fragment that’s available for all Sites, navigate to the _Global_ Site and create your Fragment there. Global Fragments are inherited by child Sites, so they can only be edited from the Global Site. Any resources the Global Fragment references (e.g., image) from the Global Site is copied to a Site that leverages the Fragment.


2. Select _Site Builder_ → _Page Fragments_

![Figure 1: Here is the Page Fragments page with no custom Fragments or Collections created.](https://liferay-support.zendesk.com/hc/article\_attachments/360040158772/empty-fragments-page.png)

Fragments are organized in _Collections_. The main Page Fragments page shows available Collections (out-of-the-box Fragment Collections to start), allows Import and Export, and enables you to create Collections. You can also manage the organization and display of Fragments and Collections you have created.

**Note:** You cannot edit a default Fragment. If you’d like to provide an edited version of a default Fragment, you can copy it to your custom Collection and edit it there. To do this, navigate to the default Fragment Collection and click the Fragment’s _Actions_ (![Actions](https://liferay-support.zendesk.com/hc/article\_attachments/360040158752/icon-actions.png)) → _Copy To_ button. Then select the Collection to copy the default Fragment to.&#x20;

To create a Fragment, you must first create a Collection.

1. Click _New_ → _Collection_ to add a Collection.
2. Give the Collection a _Name_ and _Description_ and click _Save_.

Collections help you organize Fragments, and can be used to differentiate between different types of Fragments or Fragments used by different groups or departments. Next, create a Fragment inside the Collection you created.

1. Click on the Collection you created.
2. Click the _Add_ icon (![New](https://liferay-support.zendesk.com/hc/article\_attachments/360037581112/icon-add.png)) to create a Fragment.
3. Choose either _Section_ or _Component_.
4. Give it a _Name_ and click _Save_.

The Fragment development environment appears. Each pane in the editor has a different function:

* The top left pane is for entering HTML.
* The top right pane is for entering CSS.
* The bottom left pane is for entering JavaScript.
* The bottom right pane provides a live preview as you work in the other panes.

![Figure 2: The Fragments editor provides an environment for creating all the parts of a Fragment.](https://liferay-support.zendesk.com/hc/article\_attachments/360037575151/fragments-editor.png)

In addition to standard HTML, CSS, and JavaScript, developers can also embed widgets and provide fields containing text and images that can be edited during the final Content Page publication process.&#x20;

You can also include resources in your Fragment Collection that your Fragments can reference. This is helpful when exporting/importing Fragments: their resources are automatically included. If they’re stored somewhere else (e.g., Documents and Media), you must export/import them separately. Click the _Resources_ tab for your Collection and add resources (e.g., image, document, etc.) there.

![Figure 3: The Resources tab can be selected from the Fragment Collection.](https://liferay-support.zendesk.com/hc/article\_attachments/360037581132/fragment-resources-tab.png)

Once added, you can reference resources from your Fragment’s code without worrying about their availability.&#x20;
