# Aggregation Fields

Aggregation fields perform basic calculations on the entries from a relationship table and display the resulting values. Available [aggregation functions](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/fields/aggregation-fields#functions-reference) include count, sum, average, min, and max. Each function determines how values are calculated for the field. Aggregation field values are read-only.

To include aggregation fields, an object must be on the parent side of a one-to-many relationship or on either side of a many-to-many relationship. The related object must have a numeric field (i.e., integer, long integer, decimal, or precision decimal) to use the sum, average, min, and max functions.

After adding an aggregation field, you can add [filters](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/fields/aggregation-fields#using-aggregation-filters) to it. These filters can use date, integer, or picklist fields in the related object to limit the entries included in the calculation. Only entries that meet all filter criteria are included in the aggregation field’s calculations.

### [Functions Reference](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/fields/aggregation-fields#functions-reference) <a href="#functions-reference" id="functions-reference"></a>

Aggregation fields can have these functions:

| Function | Description                                                                         |
| -------- | ----------------------------------------------------------------------------------- |
| Count    | Checks how many entries relate to the current entry and displays the total.         |
| Sum      | Retrieves a numeric value from each related entry and calculates the sum.           |
| Average  | Retrieves a numeric value from each related entry and calculates the average.       |
| Min      | Retrieves a numeric value from each related entry and calculates the lowest value.  |
| Max      | Retrieves a numeric value from each related entry and calculates the highest value. |

![Aggregation fields can use the count, sum, average, min, or max functions.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/fields/aggregation-fields/images/01.png)

Consider a scenario with two insurance objects: policies and claims. If you relate the policy object to the claims object in a one-to-many relationship, you can add these aggregation fields to the policy object:

* Count: Count the number of claims related to each policy.
* Sum: Calculate and display the total monetary value of claims related to each policy.
* Average: Calculate and display the average monetary value of claims related to each policy.
* Min: Identify and display the lowest value claim for each policy.
* Max: Identify and display the highest value claim for each policy.

### [Using Aggregation Filters](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/fields/aggregation-fields#using-aggregation-filters) <a href="#using-aggregation-filters" id="using-aggregation-filters"></a>

1. Begin editing an aggregation field.
2. Under Filters, click _Add_ (![Add Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png)).
3. Select a field to filter by. Available options include date, integer, and picklist fields in the related object.
4.  Select a _Filter Type_ to determine how the filter uses the selected field. Available options depend on the selected field.

    For date fields, select _Range_ and enter a start date and end date.

    For picklist fields, select _Exclude_ or _Include_ and check the desired picklist values.

    For integer fields, select _Is Equal To_ or _Is Not Equal To_ and enter a whole number.
5.  Click _Save_.

    ![Select a field and filter type.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/fields/aggregation-fields/images/02.png)
6.  (Optional) Repeat this process to add multiple filters to an aggregation field.

    ![Add multiple filters to an aggregation field](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/fields/aggregation-fields/images/03.png)
7. When finished, click _Save_.

Once saved, only related entries that meet all filter criteria are included in the aggregation field’s calculations.

\
