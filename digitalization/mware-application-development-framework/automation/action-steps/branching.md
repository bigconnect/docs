# Branching

The automation branching feature introduces a powerful new way to control the flow of your automation processes. Branching dramatically improves flexibility and efficiency by allowing you to create multiple conditional paths within a single automation workflow. Instead of progressing through a linear sequence of steps, you can now define different paths based on specific conditions, enabling dynamic and adaptive workflows.

With this new feature, you can create branches by clicking the Branching Icon to the left of the "+" symbol on an existing step. This creates two branching paths that originate from the selected step. Additionally, an Add Additional Branch button allows you to add more branching conditions to the right of the existing branches. Each branch operates independently, with its own set of conditions that can be configured to control the automation flow.

<figure><img src="https://files.readme.io/cbadceb425527a9d558ff3dd75b511386ea88475af70248ca3d173e70a1733e7-Screenshot_2024-11-06_at_12.43.47.png" alt=""><figcaption></figcaption></figure>

### Key features

1. **Create Multiple Conditional Paths**: You can split your automation into two or more paths based on specified conditions. This mimics the behavior of conditional statements, providing flexibility to route different inputs or events down other paths.
2. **Add Additional Branches**: Beyond just two initial paths, you can continue adding branches by clicking the Add Additional Branch button, allowing you to handle complex workflows with multiple potential outcomes.
3. **Configurable Conditions**: Each branch has its own set of conditions configured by clicking the Add Condition button. This opens a modal with a dynamic filtering form, where you can compare values (e.g., if a value is equal to or not equal to a specified parameter) to determine the path that should be followed.
4. **Order of Execution**: Arrows within the branching step allow you to change the order of conditions being evaluated. This ensures that you have control over which branch is checked first, giving you priority management for condition evaluation.
5. **Deletion and Tooltip**: Branches can be deleted when no longer needed, and a helpful tooltip explains that "Only the first branch which matches its condition will run." This ensures users understand that the evaluation stops once a condition is met, preventing unnecessary steps.

### Branching benefits

Greater flexibility: The new branching system is more efficient as it allows the automation to split into multiple paths based on conditions, reducing the need for long, repetitive chains of steps.

The new branching system solves this by allowing the automation to split into multiple paths based on dynamic conditions. You no longer need to create long chains of steps to cover every possibility, which is done by setting conditions. This reduces complexity, saves time, and offers more powerful workflow customisation.

It supports complex use cases. The ability to add numerous branches means that you can design automations that handle highly complex use cases. Whether you need to route customer requests based on various criteria, trigger different actions depending on product type, or segment workflows based on user attributes, automation branching enables this level of complexity without requiring a cumbersome setup.

### Branching Tutorial

In this tutorial, we will create a customer support ticketing system. The aim here is to use branching to route tickets to different teams based on various factors; **type**, **urgency**, and **customer priority level**. We will be using the internal data source for this tutorial

1. Create a table called Support\_tickets
2. Create 5 fields
   1. Title - text type field
   2. Ticket\_type - Single select
      1. Bug - Default to this value
      2. Feature request
      3. Enquiry
      4. Other
   3. Urgency - Single select
      1. Low - Default to this value
      2. Medium
      3. High
   4. Customer\_priority\_level - Single select
      1. Low - Default to this value
      2. Medium
      3. High
   5.  Notes - Long form text field



       <figure><img src="https://files.readme.io/1b5ec26ac16697234f8d70f820f3740a3544a1e31561f0b25f9e290fd7f62aff-Screenshot_2024-10-23_at_10.47.19.png" alt=""><figcaption></figcaption></figure>
3. Click the **Automations** button shown in the above image
4.  Click **Generate automation**



    <figure><img src="https://files.readme.io/42fa35b1f3da3386c17e4825bc7f125bb502a15ed47a7a95292f888f95aab59d-Screenshot_2024-10-23_at_10.57.01.png" alt="" width="375"><figcaption></figcaption></figure>
5.  Click **Row is created**



    <figure><img src="https://files.readme.io/91c08dd556ded6bfe243c88df55518c6c68fa118df12daebb27624ec32a2f7e3-Screenshot_2024-10-23_at_10.57.10.png" alt=""><figcaption></figcaption></figure>
6. You should now be redirected to the automations area with the correct automation type.
7. Now lets start setting up our conditional branches
   1. Click the branching icon beside the + symbol
   2. Add an extra branch (By default you start with 2)
   3. Click the Add condition button to add conditions
      1.  The first will cover bugs that are medium or high were the customer level is high



          <figure><img src="https://files.readme.io/afa714c610b9ccf6e6140c027c844f6f688a6c8e6165ee95bfb440c0f50e9cdc-Screenshot_2024-10-23_at_12.43.17.png" alt=""><figcaption></figcaption></figure>
      2.  The second will be were any other bugs go through that don't match the above criteria



          <figure><img src="https://files.readme.io/dd22831771ffca78429030bbf478953d399823bcddf7d9648895a4a3b7025b74-Screenshot_2024-11-06_at_13.20.38.png" alt=""><figcaption></figcaption></figure>
      3.  The last will handle any non bug related ticket submissions



          <figure><img src="https://files.readme.io/90f9b8b85096dd4c827376363c47b383b906a0e02594cfcad680648d802640fa-Screenshot_2024-11-06_at_13.21.14.png" alt=""><figcaption></figcaption></figure>
   4. Provide a descriptive name to each of the branches so you know what they do
8. For this tutorial I will just output a different backend log for each branch. You can add any step or run any process here e.g. email, updating a row or even triggering a different automation to handle a specific flow.
   1. Add different text to each backend log
9. Head back to your data area and click screens in your table
10. Click **Generate app screen**



    <figure><img src="https://files.readme.io/4df3396071cedc1e26d693fa15e3bf5051efd06e9f6a441ac64a21237817f644-Screenshot_2024-10-23_at_13.04.45.png" alt="" width="375"><figcaption></figcaption></figure>
11. Click table



    <figure><img src="https://files.readme.io/3521d43eecd5d2275cc951d55aa8a90db70876e85e7ed4e7168fc2c883aebec6-Screenshot_2024-10-23_at_13.04.55.png" alt=""><figcaption></figcaption></figure>
12. Click side panel and then click done



    <figure><img src="https://files.readme.io/ef453791a4581ceb125894f44678482b5ad3934929d3892320d57972b389be7c-Screenshot_2024-10-23_at_13.05.01.png" alt="" width="563"><figcaption></figcaption></figure>
13. This will bring you to an app screen were it has auto generated a screen.
14. Click publish at the top right of your screen and then click the button publish inside
15. Navigate to your published app and create 3 different records
    1. The first a bug that is High and is for a high customer. This should go output the first branches backend log
    2. Create a bug thats high but for a medium customer. This should go output the second branches backend log
    3. Create a support\_type of anything other than a bug. This should go output the last branches backend log

#### End result

<figure><img src="https://files.readme.io/35deef984ec89d7e03f2dca0602d6b48de80919a23dd598417958687edf57846-20241106_130833.gif" alt=""><figcaption></figcaption></figure>
