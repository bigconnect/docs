# Multi-select

In Budibase, there are two data types that allow a value to be set from a list of options.

The first is the Options type, which is compatible with the _Text_ data type, and only allows a single option to be chosen.

The **Multi-select** type is similar to the options type, but it allows you to select multiple options, and is not convertible to any other type. You can still select a single option or none at all.

A common use of the multi-select data type is to pair it with the Multi-select picker in your Forms.

<figure><img src="https://files.readme.io/1763d01-Screenshot_2023-02-17_at_11.01.51.png" alt="Multi-select column"><figcaption><p>Multi-select column</p></figcaption></figure>

<figure><img src="https://files.readme.io/78651d6-Screenshot_2023-02-17_at_11.03.02.png" alt="Selecting multiple options"><figcaption><p>Selecting multiple options</p></figcaption></figure>

When a multi-select value is saved to a data table, the values will be presented in individual coloured pills:

<figure><img src="https://files.readme.io/eb341a7-Screenshot_2023-06-29_at_09.12.50.png" alt="Multi-select row presenting two selected options"><figcaption><p>Multi-select row presenting two selected options</p></figcaption></figure>

\


### Filtering

A number of array based [filters](https://docs.budibase.com/docs/searchfilter-data) are available when using a multi-select.

#### Contains

> The multi-select value must contain all of the selected comparison options.

| Filter                                                                        | Result                                                             |
| ----------------------------------------------------------------------------- | ------------------------------------------------------------------ |
| ![](https://files.readme.io/317c9e3-Screenshot\_2023-02-17\_at\_11.34.16.png) | ![](https://files.readme.io/41c2407-contains\_senior.png)          |
| ![](https://files.readme.io/5369cde-Screenshot\_2023-02-17\_at\_11.33.10.png) | ![](https://files.readme.io/b0afbc7-contains\_senior\_manager.png) |

\


#### Does not contain

> The multi-select value must not contain all of the selected comparison options.

**Single value**

| Filter                                                                        | Result                                               |
| ----------------------------------------------------------------------------- | ---------------------------------------------------- |
| ![](https://files.readme.io/e412aa2-Screenshot\_2023-02-17\_at\_11.41.56.png) | ![](https://files.readme.io/152c925-not\_senior.png) |

**Multiple values**

Note that in this case only rows that have both _Senior_ AND _Manager_ are filtered out.

| Filter                                                                        | Result                                                        |
| ----------------------------------------------------------------------------- | ------------------------------------------------------------- |
| ![](https://files.readme.io/17641c9-Screenshot\_2023-02-17\_at\_11.48.05.png) | ![](https://files.readme.io/435ea25-not\_senior\_manager.png) |

\


#### Has any

> The multi-select value contains any of the selected comparison options.

<figure><img src="https://files.readme.io/81b44e9-Screenshot_2023-02-17_at_11.52.37.png" alt="Has any: Senior or Manager"><figcaption><p>Has any: Senior or Manager</p></figcaption></figure>

<figure><img src="https://files.readme.io/5ce87dd-has_any.png" alt="Result"><figcaption><p>Result</p></figcaption></figure>
