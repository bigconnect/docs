# Dynamic filter

The Dynamic Filter component is a powerful tool that allows you to filter data from your Data Provider's data set. This is accessible in both Data View and Design View, making it highly versatile for customising your app to display relevant data effectively.

### Data view

You can no longer apply filters to the base table. Instead, you must create a view to apply filters to. To help you quickly locate specific data, the Data View in your  app includes a Filter button at the top right of each view. Clicking this button opens the Dynamic Filter, allowing you to apply advanced filtering criteria to data within your views for faster, more targeted access.

<figure><img src="https://files.readme.io/a27420156d9c907d5e56ec2c98f16f4e7c049ffa52bb8e6b75a4971f3fe5ae5e-Screenshot_2024-11-04_at_11.48.18.png" alt=""><figcaption></figcaption></figure>

### Adding and Configuring Filters

To add a filter, click the Add Filter button. This will open a context menu, allowing you to customise your filters to display your data. The settings within this context menu are as follows;

1.  **Initial Group Setting**: Select whether to display data matching all or any of the filter groups you create. Choosing all will apply an AND condition between groups, while any will apply an OR condition.



    <figure><img src="https://files.readme.io/626801031a7cac1dd1950a7105a2125494680abed426728e948784ecbb7ed8ac-Screenshot_2024-11-04_at_11.54.26.png" alt=""><figcaption></figcaption></figure>
2.  **Adding Filter Groups**: Click **Add filter group** to create multiple groups, each with its own criteria. This setup allows you to build complex filtering logic for refined results.



    <figure><img src="https://files.readme.io/e3827bdae4ff4923b0e39282af3f41c386cff59e3a70889c290752a9bc07f501-Screenshot_2024-11-04_at_11.53.25.png" alt=""><figcaption></figcaption></figure>
3. **Group-level Matching**: Inside each group, choose whether to match all filters or any filters within that group, giving you control over AND/OR conditions within each specific group.
4.  **Empty Filter Option**: A new setting lets you decide how to display data when no filters are applied. If filters are empty, you can choose to display all rows or none, allowing for greater control over initial view settings.



    <figure><img src="https://files.readme.io/96d3c42f95bca03be0975fa54b1a7a99113393eaff2fcb01bd4527a3da4486f4-Screenshot_2024-11-04_at_11.54.54.png" alt=""><figcaption></figcaption></figure>

#### Filters

In each filter, you can define the conditions that data must meet to appear in the results. Each filter has three configurable fields:

*   **Column**: Choose the column you wish to filter.\


    <figure><img src="https://files.readme.io/47ffe88a95d472eb511ae766906ab689df58e3cdf773efdd283b7c2ca099047e-Screenshot_2024-11-04_at_11.58.12.png" alt=""><figcaption></figcaption></figure>
*   Operator: Select the appropriate operator (e.g., equals, contains)\


    <figure><img src="https://files.readme.io/a66ab1435820d3b0fa7f34133d9d7b3f0c685c55bcde14f77f17eafd83888ac1-Screenshot_2024-11-04_at_11.58.20.png" alt=""><figcaption></figcaption></figure>
*   The last input field is the value you search against. (An example of this in action can be seen in the gif below.)\


    <figure><img src="https://files.readme.io/45581fa20bb752bf221508e92ac0a5d868e0dd0e941ed73aa5c2ef76395e108a-20241104_120730.gif" alt=""><figcaption></figcaption></figure>

### Design view

The Dynamic Filter component functions the same way in Design View as in Data View but offers additional flexibility when used alongside the Data Provider's component.

#### Quick setup process

* Add a Card Block component and then eject it to enable customisation.
* Add the Dynamic Filter as a child component within the Data Provider's component
* Users can then configure the filters accordingly and pull out relevant information they require

This dynamic approach makes it easy to customise data display based on complex filtering conditions, whether working in Data View or Design View.

<figure><img src="https://files.readme.io/9b5682f82874dc12e6e53b00f7b654d598992db9faeaf3aa1c0f187f309674cd-20240911_125205.gif" alt=""><figcaption></figcaption></figure>
