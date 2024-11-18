# Row actions

A Row Action is configured against the table but can be enabled or disabled per view. This allows you to control which users can trigger specific workflow stages.

Using row actions allows you to securely ensure that only specific users with the correct permissions can progress row data through a pipeline

### Setup

Below are the steps on how you can setup your first row action.

1. Go to the data and select the table/view you wish to create a row action on
2.  Click the **Row actions** button beside **Access** above your table



    <figure><img src="https://files.readme.io/492e4e5ed4e348ae7cab59120fc199897ab481b18d297e908af368ae094b1fb9-Screenshot_2024-10-18_at_09.23.40.png" alt=""><figcaption></figcaption></figure>
3. A context menu will appear, providing you with some information about row actions
4. Click the **Create row action** button
5.  You will be prompted with a modal. Type a relevant name for the row action e.g. accept, deny, assign etc



    <figure><img src="https://files.readme.io/55de99af29b3983cd2bb9784c43aaa488149fd3a54474280c9fd729412b3ead1-Screenshot_2024-10-18_at_09.27.59.png" alt=""><figcaption></figcaption></figure>
6. Click the **Create** button
7.  You will now be redirected to the row action automation. You can begin building out your workflow for this row action here



    <figure><img src="https://files.readme.io/55a35b744fe6d173be281e2d3e190fc70e37d0e75dc957b223a40892bb49ac26-Screenshot_2024-10-18_at_09.28.10.png" alt=""><figcaption></figcaption></figure>
8.  Returning to your table, you can see how many row actions have been created for a table. You can also create views with relevant access roles. This will carry over to your row actions. If a certain user doesn't have the correct permissions, they won't have access to the row action. These can be managed on a view-per-view basis by toggling them on and off.



    <figure><img src="https://files.readme.io/025f4ea98ad6234596556b2ce62f139bcdbc78c039fa43fd8f242a15a81ec41c-Screenshot_2024-10-18_at_13.39.58.png" alt=""><figcaption></figcaption></figure>
9.  After toggling these on, you should see a new dropdown button on the table's right-hand side.



    <figure><img src="https://files.readme.io/3284496082e39430479dac4f3746654f75dd98ba603e008716cab939b6424c17-Screenshot_2024-10-21_at_08.07.41.png" alt=""><figcaption></figcaption></figure>
10. Clicking on the dropdown button on the far right will display all available row actions. You can then click one of the row actions to trigger the corresponding workflow.



    <figure><img src="https://files.readme.io/68bd1b80640a5c19d4a261c5d65971bccc7afbb820af583f4a096a337de67484-Screenshot_2024-10-22_at_13.14.04.png" alt=""><figcaption></figcaption></figure>

### Design

A new action with the same name has been added to trigger these row actions.

From the design perspective, there are two ways to quickly add row actions to your components.

The first is with a table component. Start by adding a table component to your design area. Select the table component, scroll to the bottom of its settings, and click `Add button`. This will let you choose either a custom button or a row action. The button title will match the name of your row action.

<figure><img src="https://files.readme.io/b24adf24e25e76f4b67ea5f55ac109aef0aec4a8ebb9d510acf60152dd50ee90-Screenshot_2024-10-21_at_09.37.08.png" alt=""><figcaption></figcaption></figure>

When selected, a new button will appear in the table. You can review the button's settings to see how it’s configured. There is also an option to make the button collapsible, which is helpful when you have numerous buttons attached to the table, as they will be grouped into a dropdown menu.

Once everything is set up, users can use these buttons to trigger your row action automation, allowing users to process your workflows.

#### Form block

A form block can also generate buttons with row actions. The setup here is even easier than the table, as it will auto-generate the button based on your selected schema. Ensure you use an update form type, as row actions are meant to progress existing data through a workflow.

<figure><img src="https://files.readme.io/92e56192ea887b89064190a13ac6b8a7ece55a1957c3577712558a9843776616-20241021_132924.gif" alt=""><figcaption></figcaption></figure>

You can also add a Row action to any component that allows actions to be applied.
