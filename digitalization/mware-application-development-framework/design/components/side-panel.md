---
description: Show/Hide content without a modal screen
---

# Side panel

A side panel is a special container that slides in from the right-hand side when opened.

You have a wide variety of different components that can be added to the side panel. There are a few that aren't allowed to be nested inside. Another side panel, modal and a section.

<figure><img src="https://files.readme.io/031b547-Screenshot_2022-12-12_at_13.19.04.png" alt="Side panel with nested form block"><figcaption><p>Side panel with nested form block</p></figcaption></figure>

<figure><img src="https://files.readme.io/bb8a36a3b2d1186b636098d34f14a0a691f5808b1e39a35351a6971d1d7d3862-Screenshot_2024-09-30_at_08.35.41.png" alt="App preview of a side panel"><figcaption><p>App preview of a side panel</p></figcaption></figure>

#### Opening a side panel

Side panels are opened via the **Open Side Panel** Action. This could be triggered from a button click, or some other event such as on change.

<figure><img src="https://files.readme.io/e594143-Screenshot_2022-12-12_at_13.47.45.png" alt=""><figcaption></figcaption></figure>

You can choose to open any side panel that is anywhere in the screen component tree.

#### Closing a side panel

A side panel can be closed by a user simply clicking an area outside of the panel, or by clicking the close arrow.

<figure><img src="https://files.readme.io/ba370c4-Screenshot_2022-12-12_at_13.50.02.png" alt="Close side panel"><figcaption><p>Close side panel</p></figcaption></figure>

Alternatively, the **Close Side Panel** action can be used.

<figure><img src="https://files.readme.io/db12848-Screenshot_2022-12-12_at_13.52.29.png" alt=""><figcaption></figcaption></figure>

No screen needs to be selected as this action will automatically close any currently open side panel.

### Side panel settings

The side panel component itself has some settings.

* **Ignore outside clicks**—when checked, this prevents the side panel from closing unless a button action is clicked or the closing action shown above is clicked.
* **On Close** - This setting allows you to run actions when the side panel is closed. For example, you could run an action that refreshes a data provider or update a state value etc.

> ### 👍Table block
>
> The Table block supports built-in side panels for quickly viewing row detail or creating new entries.

#### App state & Side panel

When using the side panel you may want to create/update App state for use by a component nested within. An example of this would be; the Form / Form Block components, app state can be passed into these. The aim of this is to prevent users from jumping between pages and providing a more enriched interactive experience.

Additionally you can use the side panel as an area which contains additional information of the selected item.

**Example**

**Form usage**

1. Add the table component with a related data provider (for this I will be using the inbuilt test data).
2. Add the side panel component.
3. Within the table component `ON ROW CLICK` add 2 different actions: `Update state` and `Open side panel`\
   ![On row click button within the table component](https://files.readme.io/507f02f-small-Screenshot\_2023-05-16\_at\_13.59.53.png)
4.  Add a key name, I've used selectedRow and updated the value with `{{ Clicked row._id }}`.\


    <figure><img src="https://files.readme.io/a1d76db4d23f695b9893b44ea6d4e2317cbcdd249afe539f6caa501b0067af58-Screenshot_2024-09-30_at_08.37.03.png" alt=""><figcaption></figcaption></figure>
5. Add a Form / Form Block for this example I will be using the Form Block
6. Change the form to update/read only, then add the selectedRow app state to the `Row ID`

A similar approach to the above can be done for simply displaying additional information.

Follow the previous steps until `Step 5`. (Please ensure the side panel along with the newly created data provider are nested within the parent data provider.) From here we are going to add a Data provider and nested within that a Repeater. From here click on your newly created Data provider, then click `Define Filters`

<figure><img src="https://files.readme.io/893b02dccad7d220776e250179e050ed7e8c421e67ed85a91643d826b255de75-Screenshot_2024-09-30_at_08.47.50.png" alt=""><figcaption></figcaption></figure>

Add a filter which uses the parent Data provider (limits the outgoing requests). Start building out your side panel area to include additional content you wish to show on click.

<figure><img src="https://files.readme.io/b982f71af1755c262bc27f76b1109c29c8608d1e2d8d20871e37e188ddb10fae-20240930_084644.gif" alt=""><figcaption></figcaption></figure>
