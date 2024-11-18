# Line Chart

The Line Chart component is an easy way to visualize progression easily. For example, if you want to plot the amount of applications you have received per day, you could get something like this.

<figure><img src="https://files.readme.io/cd70c88-applications-per-day.png" alt=""><figcaption></figcaption></figure>

### Adding a Line Chart

To add a line chart, head over to the design section in the UI. Then, you'll need a wrapping [Data Provider](https://docs.budibase.com/docs/data-provider). After you've added the data provider, add a `Chart > Line Chart` to the design as a child of the data provider. This should connect your chart to the database directly.

<figure><img src="https://files.readme.io/c5b76f9-Screenshot_2023-07-04_at_13.41.35.png" alt=""><figcaption></figcaption></figure>

Then, in the Settings Panel you will be able to configure your chart to display exactly what you want. First, you want to select the correct [Data Provider](https://docs.budibase.com/docs/data-provider), if you have more than one as a parent. The second step is to select a `Label column`. The Label Column defines the labels on your X-axis. Lastly, you'll want to select a `Data column`. This column will represent the data drawn in the chart. For this to work the column will of course have to be numeric.

And that's it, you have your line chart. Want to understand all the possible customization settings? Check the reference below.

> ### 📘Numeric data only
>
> Of course, line charts need to have a scale, so for the data column(s), you can only select [Numeric](https://docs.budibase.com/docs/number) columns (eg; containing values that can be converted to numbers)
>
> ![](https://docs.budibase.com/docs/line-chart)

### Settings panel

The settings panel has quite a few customization options, allowing you to get the most out of your line chart.

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

### Line Chart curve types

There are three kinds of curves you can choose from in a line chart. In the image below you can see all three, with the same data-set, as a comparison.

<figure><img src="https://files.readme.io/4b2560a-linechart-types.png" alt=""><figcaption></figcaption></figure>
