# Multi-step form block

When building a Form with many Form steps the Component Tree can get quite long and tricky to maintain. The multi-step form block aims to solve this problem by allowing you to manage your form steps within the Settings Panel.

\


### General settings

When you initially create a multi-step form block, it will have a single step displaying all the form fields for the chosen _Data_ source. Blocks support the internal DB tables, as well as SQL tables.

Just like a Form block you may change the form _Type_ between 'Create', 'Update', and 'View' as needed.

The selected data source and form type will be applied across all form steps in your multi-step form block.

<figure><img src="https://files.readme.io/84c1b7c-Screenshot_2024-01-10_at_16.26.16.png" alt="General settings"><figcaption><p>General settings</p></figcaption></figure>

\


### Details

The details section includes settings that will be applied on a per step basis. Initially there will be a single step, but you can click to `Add step` button to get started.

<figure><img src="https://files.readme.io/dbba438-add_step.png" alt="Add a form step"><figcaption><p>Add a form step</p></figcaption></figure>

Once another form step has been added, you will be able to navigate between the steps using arrow buttons. The `x` button will delete a form step, while the `+` icon will add another step immediately after the currently selected step.

<figure><img src="https://files.readme.io/70e740a-step_navigation.png" alt="Navigating between form steps"><figcaption><p>Navigating between form steps</p></figcaption></figure>

**Title and description**

By default the title of each step will be tied to current step Binding like so:

```
Step {{ Multi-step Form Block.Current Step }}
```

This will conveniently label each step "Step 1", "Step 2", etc without needing manual input. With that said, you can easily replace this with your own step title.

Similarly you can add a _Description_ which will appear below the step title, however this is blank by default.

<figure><img src="https://files.readme.io/73a95e9-description.png" alt="Adding a description"><figcaption><p>Adding a description</p></figcaption></figure>

<figure><img src="https://files.readme.io/6cfaf92-Screenshot_2024-01-10_at_16.16.46.png" alt="Step title with description"><figcaption><p>Step title with description</p></figcaption></figure>

**Fields**

Form fields can be shown/hidden and further configured for each form step. For example you may want to show only 'contact information' fields in your first step, while showing only 'address details' fields in the second step.

Read configuring form fields for more information.

**Buttons**

When you add new steps in the multi-step form block, Mware Application Development Framework will intelligently configure the buttons you would likely want as follows:

| First step                                     | Middle step                                     | Last step                                     |
| ---------------------------------------------- | ----------------------------------------------- | --------------------------------------------- |
| ![](https://files.readme.io/8885999-first.png) | ![](https://files.readme.io/0159d5e-middle.png) | ![](https://files.readme.io/fead5f6-last.png) |

These buttons will use the Change Form Step action to appropriately navigate between the different steps, and the 'Save' button will naturally save your form at the end. If you need custom actions, click on the cog icon beside a button. You may also add and remove buttons as needed.

Read configuring form buttons for more information.
