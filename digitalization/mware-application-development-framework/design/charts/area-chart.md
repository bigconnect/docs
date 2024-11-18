# Area Chart

The area chart is a mixture between the Bar Chart and the Line Chart. Allowing you to display multiple columns of data on top of each other, having the area coloured.

<figure><img src="https://files.readme.io/95d9b99ce89a358053d46ee53b6ddd0f6091137203bdc492e3dc5be108ec18b8-Screenshot_2024-10-28_at_13.11.54.png" alt=""><figcaption></figcaption></figure>

Not only can you stack the different data like above, but you can also let them overlay.

### Adding an Area Chart

To add the area chart to your UI, you first need to wrap it in a [Data Provider](https://docs.budibase.com/docs/data-provider). Then you can add the Area chart to your UI through `Chart > Area Chart`.

![](https://files.readme.io/451ee80-Screenshot\_2023-07-04\_at\_13.40.21.png)

### Displaying data

After adding the Area chart to your UI, you can now select data. You should've already selected a data source through the [Data Provider](https://docs.budibase.com/docs/data-provider), so now you can select the Data Provider in the Area chart.

In the chart on top of this page there are 4 columns selected, and they stack by default. To do this, just select the data provider, a label column, and select the 4 columns you want to display.

![](https://files.readme.io/727e816066d50046ce2c59394f6f9bf093948188545ee29a4bbe5c19ee3ea7ae-Screenshot\_2024-10-28\_at\_13.10.28.png)

And that is all you need to do to get an area chart set up. But of course, you customize much more in the Settings Panel. To read more about further customization, check the reference table below.

> ### 📘Numeric data only
>
> Of course, line charts need to have a scale, so for the data column(s), you can only select [Numeric](https://docs.budibase.com/docs/number) columns (eg; containing values that can be converted to numbers)

### Settings panel

The settings panel has quite a few customization options, allowing you to get the most out of your bar chart.

| Setting                             | Description                                                                                                                                                                                                                                                                                                                                              |
| ----------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Provider                            | The Data Provider as a parent of your chart                                                                                                                                                                                                                                                                                                              |
| Label Column                        | The Labels you want to display with your data. These are displayed on the X-axis                                                                                                                                                                                                                                                                         |
| Data Column(s)                      | The data you want to display as lines. This has to be numeric. Can be multiple                                                                                                                                                                                                                                                                           |
| Format                              | Will divide labels by thousand or million on the axis and adds a `K` or `M` to the axis respectively                                                                                                                                                                                                                                                     |
| <p>Y-axis Label<br>X-axis label</p> | The labels you want to display next to the Y and X-axis.                                                                                                                                                                                                                                                                                                 |
| <p>Width<br>Height</p>              | The size you want the chart to be, supports all CSS supported sizes                                                                                                                                                                                                                                                                                      |
| Colours                             | Choose from one of the predefined colour-palettes                                                                                                                                                                                                                                                                                                        |
| Curve                               | <p>The type of line you want to have.<br><strong>Smooth</strong>: A curved line flowing with the data<br><strong>Straight</strong>: A line straight from point to point<br><strong>Stepline</strong>: A line which remains horizontal, and drops down completely for the next data point. See below the reference table for an example of all three.</p> |
| Data Labels                         | Will display the value on every point on the graph                                                                                                                                                                                                                                                                                                       |
| Animate                             | Will animate when the data changes or on startup. Data-change can happen through Dynamic filter's for example.                                                                                                                                                                                                                                           |
| Legend                              | Will display, in the upper-right corner, the label for each of the data columns displayed on the chart                                                                                                                                                                                                                                                   |
| Stacked                             | <p>Check this setting to have the different data columns stacked, which means they won't overlay but be stacked on top of each other.<br>You can see the difference between stacked and not stacked below the reference table</p>                                                                                                                        |

#### Stacked vs non-stacked comparison

<figure><img src="https://files.readme.io/f5eb085-area-stacked-comparison.png" alt="1958"><figcaption><p>Not stacked (left) and Stacked (right) comparison</p></figcaption></figure>
