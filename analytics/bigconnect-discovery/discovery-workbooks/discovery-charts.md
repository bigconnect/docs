# Discovery Charts

Charts that analyze and visualize data are the main components of a dashboard. This section describes some concepts that you need to understand to create a chart for data analytics, as well as the elements that make up the chart configuration UI.

The chart home is divided into the following three sections:

![](../../../.gitbook/assets/image%20%2869%29.png)

1. **Column/chart selection section:** This section is so organized that you can create a chart step by step. You can either choose columns under the Data menu to have appropriate chart types suggested, or select a chart type under the Chart menu before choosing data columns. In addition, you can configure some analytics settings under the Analytics menu.
2. **Visualization section:** This section is composed of the shelves onto which columns are put and the visualization area where the chart is displayed. Once data and a chart type are selected in the column/chart selection section, the chart is drawn in this area.
3. **Option section:** Used to customize the appearance and display of the chart. Depending on the chart type, the option section may include the filter, palette, axis, numeric format, and chart format areas.

In the subsequent subsections, we will explain how to use this user interface to create and manage various types of charts.

## Data column list

The columns listed in the data column list are categorized into “dimensions” and “measures.” For the concept of dimensions and measures, refer to **“Dimensions” and “Measures”**.

In the data column list, you can view and edit information on connected data sources, as well as add or remove column filters easily.

![](../../../.gitbook/assets/image%20%2876%29.png)

1. **Select/set data source:** Allows you to select a data source or configure its associations and joins.
2. **Data details:** Click on it to pop up a dialog box displaying information about the selected data source.
3. **Search by column name:** Searches the column list by name.
4. **Add custom column:** Click on it to open the dialog box to create a new column by combining/processing data source columns. Custom columns are commonly used throughout the dashboard.
5. **Apply/clear filter:** Hover the mouse over a column to display this button. Click on it to apply a chart filter to the column, and click again to clear the chart filter. For columns to which a filter is applied, the ![more](https://metatron-app.github.io/metatron-doc-discovery/en/_images/chart_icon_more.png) icon is displayed regardless of the mouse position.
6. **More:** Hover the mouse over a column to display this button. It is used to check additional information on the column and set an alias.

   > * ![info](https://metatron-app.github.io/metatron-doc-discovery/en/_images/chart_icon_info.png) : Click on it to pop up a dialog box displaying a summary of the column and its data values.
   > * **Logic column name:** Shows the logical name of the column.
   > * **Type:** Shows the logical type of the column.
   > * **Alias:** Sets a column alias. A regular column name can contain only alphanumeric characters and a limited number of special characters with no spaces allowed. Therefore, setting an alias may help to identify the column for convenient analytics work. Aliases are commonly used throughout the dashboard.
   > * **Value alias:** You can also set an alias for each data value in the column. Aliases are commonly used throughout the dashboard.

## Adding a custom column

Click the + button on the data source column list to open a dialog box for adding a custom column. By applying various formulas to existing columns of the data source, you can create a new column that helps create your desired chart.

![](../../../.gitbook/assets/image%20%28221%29.png)

![](../../../.gitbook/assets/image%20%28420%29.png)

1. **Column name:** Fill in a name for the custom column.
2. **Coding box:** Write a code for the custom column. Click a list from the column or formula list below to type your selection in this box automatically.
3. **Add column:** Lists the columns of the data source. Click a column in the list to automatically type your selection in the coding box.
4. **Add formula:** Lists the formulas supported by Discovery. Click a formula in the list to type your selection in the coding box automatically, with the text cursor relocated to where a parameter needs to be inserted. For details on each formula’s purpose, use, and examples, see the help box on the right.

## Draw a chart \(pivoting\)

Pivoting is a process of grouping the given table by specific columns, thereby helping the analyst view particular aspects of the source data in a graphic or tabular chart. This process includes selecting columns that contain meaningful data and placing them on the column/row/cross shelves.

![](../../../.gitbook/assets/image%20%28462%29.png)

In the example shown above, two dimension columns are placed on the column shelf and one measure column is placed on the cross shelf. The chart displays data resulting from the columns placed on the shelves in this way.

Mandatory/recommended column types for each shelf vary depending on the chart type. Selecting a chart type before placing columns on a shelf shows the necessary column types for each shelf.

![](../../../.gitbook/assets/image%20%28434%29.png)

### Column/row/cross shelves

Think of the structure of Excel to understand what column/row/cross shelves work for. As shown below, the crossing of each column and row cross contains a value.

![](../../../.gitbook/assets/image%20%28323%29.png)

Whereas Excel shows data in a two-dimensional grid composed of columns, rows and crosses, BigConnect Discovery is an OLAP data discovery tool capable of multidimensional data representation. In the following Discovery chart, the column, row, and crossing axes form a three-dimensional cube.

![](../../../.gitbook/assets/image%20%28399%29.png)

If the values of an Excel grid are displayed in a three-dimensional chart, each crossing value will be represented by a bar. However, Discovery needs to display such a chart two-dimensionally; for this, bars either in the same column or in the same row get stacked at one point while remaining distinctive from one another. The resulting two-dimensional chart is shown in the gray area of the chart below.

![](../../../.gitbook/assets/image%20%28396%29.png)

## Type of charts

BigConnect Discovery provides about 20 types of charts. If you place columns on shelves before selecting a chart, suitable charts are highlighted in purple.

![](../../../.gitbook/assets/image%20%28303%29.png)

The table below summarizes conditions to create, uses, and examples for each chart.



<table>
  <thead>
    <tr>
      <th style="text-align:center">Chart name/icon</th>
      <th style="text-align:left">Conditions to create</th>
      <th style="text-align:left">Characteristics</th>
      <th style="text-align:left">Uses</th>
      <th style="text-align:left">Examples</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:center">
        <p>
          <img src="https://metatron-app.github.io/metatron-doc-discovery/en/_images/Chart_Types_Icon01.png"
          alt="icon01" />
        </p>
        <p>Bar chart</p>
      </td>
      <td style="text-align:left">Column: 1 or more dimensions / Cross: 1 or more measures</td>
      <td style="text-align:left">Compares the value of each item.</td>
      <td style="text-align:left">Used to compare groups or view trends over time. Very effective when the
        trend is significantly fluctuating.</td>
      <td style="text-align:left">Comparison between products regarding their sales and profits</td>
    </tr>
    <tr>
      <td style="text-align:center">
        <p>
          <img src="https://metatron-app.github.io/metatron-doc-discovery/en/_images/Chart_Types_Icon02.png"
          alt="icon02" />
        </p>
        <p>Table</p>
      </td>
      <td style="text-align:left">Column or row: 1 or more dimensions / Cross: 1 or more measures</td>
      <td
      style="text-align:left">Displays the values of crossings between two dimensions as text.</td>
        <td
        style="text-align:left">Used to view measure values aggregated by certain criteria. Useful to
          check exact values rather than a visualization of them.</td>
          <td style="text-align:left">Sales details by year</td>
    </tr>
    <tr>
      <td style="text-align:center">
        <p>
          <img src="https://metatron-app.github.io/metatron-doc-discovery/en/_images/Chart_Types_Icon03.png"
          alt="icon03" />
        </p>
        <p>Line chart</p>
      </td>
      <td style="text-align:left">Column: 1 or more dimensions / Cross: 1 or more measures</td>
      <td style="text-align:left">Displays data changes over time.</td>
      <td style="text-align:left">Used to view trends over time. If changes are moderate, a line chart is
        more effective than a bar chart.</td>
      <td style="text-align:left">Monthly sales trend</td>
    </tr>
    <tr>
      <td style="text-align:center">
        <p>
          <img src="https://metatron-app.github.io/metatron-doc-discovery/en/_images/Chart_Types_Icon04.png"
          alt="icon04" />
        </p>
        <p>Scatter chart</p>
      </td>
      <td style="text-align:left">Column: 1 measure / Row: 1 measure / Cross: 1 or more dimensions</td>
      <td
      style="text-align:left">Displays relations between items.</td>
        <td style="text-align:left">Used to define relations between two parameters.</td>
        <td style="text-align:left">Relations between product sales and profits</td>
    </tr>
    <tr>
      <td style="text-align:center">
        <p>
          <img src="https://metatron-app.github.io/metatron-doc-discovery/en/_images/Chart_Types_Icon05.png"
          alt="icon05" />
        </p>
        <p>Heatmap</p>
      </td>
      <td style="text-align:left">Column or row: 1 or more dimensions / Cross: 1 or more measures</td>
      <td
      style="text-align:left">Displays the values of crossings between two dimensions in colors and
        sizes at different points.</td>
        <td style="text-align:left">Used to provide an intuitive view of relations between two dimensions
          represented by colors and sizes. Similar to a table chart, but more of
          a visual type.</td>
        <td style="text-align:left">Sales of each product by region</td>
    </tr>
    <tr>
      <td style="text-align:center">
        <p>
          <img src="https://metatron-app.github.io/metatron-doc-discovery/en/_images/Chart_Types_Icon06.png"
          alt="icon06" />
        </p>
        <p>Pie chart</p>
      </td>
      <td style="text-align:left">Cross: 1 or more dimensions, 1 or more measures</td>
      <td style="text-align:left">Shows how much each item accounts for.</td>
      <td style="text-align:left">Used to compare the compositions of something.</td>
      <td style="text-align:left">Comparison between web browsers regarding their market share</td>
    </tr>
    <tr>
      <td style="text-align:center">
        <p>
          <img src="https://metatron-app.github.io/metatron-doc-discovery/en/_images/Chart_Types_Icon07.png"
          alt="icon07" />
        </p>
        <p>Map view</p>
      </td>
      <td style="text-align:left">Layer shelf: dimension (location attribute), 1 or more dimensions, 1 or
        more measures</td>
      <td style="text-align:left">Displays the data for each location on the map.</td>
      <td style="text-align:left">Used for intuitive comparisons of variables by using colors for each region.
        Used to emphasize visual elements.</td>
      <td style="text-align:left">Comparison of sales of each product by region</td>
    </tr>
    <tr>
      <td style="text-align:center">
        <p>
          <img src="https://metatron-app.github.io/metatron-doc-discovery/en/_images/Chart_Types_Icon08.png"
          alt="icon08" />
        </p>
        <p>KPI</p>
      </td>
      <td style="text-align:left">Cross: 1 or more measures</td>
      <td style="text-align:left">Displays main indicators along with their trends.</td>
      <td style="text-align:left">Used to quickly convey information on an organization&#x2019;s current
        achievement.</td>
      <td style="text-align:left">An organization&#x2019;s performance index, such as how many customers
        have been brought in this year</td>
    </tr>
    <tr>
      <td style="text-align:center">
        <p>
          <img src="https://metatron-app.github.io/metatron-doc-discovery/en/_images/Chart_Types_Icon09.png"
          alt="icon09" />
        </p>
        <p>Box plot</p>
      </td>
      <td style="text-align:left">Column: 1 or more dimensions / Row: 1 dimension / Cross: 1 measure</td>
      <td
      style="text-align:left">Indicates increase and decrease in value.</td>
        <td style="text-align:left">Used to compare groups regarding their share.</td>
        <td style="text-align:left">Proportion of flight delay accounted for by each airplane model</td>
    </tr>
    <tr>
      <td style="text-align:center">
        <p>
          <img src="https://metatron-app.github.io/metatron-doc-discovery/en/_images/Chart_Types_Icon10.png"
          alt="icon10" />
        </p>
        <p>Waterfall chart</p>
      </td>
      <td style="text-align:left">Column: 1 time-dimension / Cross: 1 measure</td>
      <td style="text-align:left">Displays cumulative changes resulting from the increase or decrease in
        value for each time interval.</td>
      <td style="text-align:left">Used to emphasize increase and decrease in value over time.</td>
      <td style="text-align:left">Changes in the number of team members for a certain period; stock price
        trends</td>
    </tr>
    <tr>
      <td style="text-align:center">
        <p>
          <img src="https://metatron-app.github.io/metatron-doc-discovery/en/_images/Chart_Types_Icon11.png"
          alt="icon11" />
        </p>
        <p>Word cloud</p>
      </td>
      <td style="text-align:left">Cross: 1 or more dimensions, 1 measure</td>
      <td style="text-align:left">Displays words sized in proportion to the number of mentions.</td>
      <td
      style="text-align:left">Used to summarize and emphasize important words.</td>
        <td style="text-align:left">Summary of the voices of customers</td>
    </tr>
    <tr>
      <td style="text-align:center">
        <p>
          <img src="https://metatron-app.github.io/metatron-doc-discovery/en/_images/Chart_Types_Icon12.png"
          alt="icon12" />
        </p>
        <p>Combo chart</p>
      </td>
      <td style="text-align:left">Column: 1 or more dimensions / Cross: 2?4 measures</td>
      <td style="text-align:left">Compares data by combining bar and line charts.</td>
      <td style="text-align:left">Used to visualize different types of data simultaneously.</td>
      <td style="text-align:left">Simultaneous monitoring of product price and sales</td>
    </tr>
    <tr>
      <td style="text-align:center">
        <p>
          <img src="https://metatron-app.github.io/metatron-doc-discovery/en/_images/Chart_Types_Icon13.png"
          alt="icon13" />
        </p>
        <p>Treemap</p>
      </td>
      <td style="text-align:left">Column: 1 dimension / Row: 1 or more dimensions / Cross: 1 measure</td>
      <td
      style="text-align:left">Displays hierarchical data using nested rectangles.</td>
        <td style="text-align:left">Used to visualize hierarchical data.</td>
        <td style="text-align:left">Monitoring of sales of products classified into major, medium, and minor
          categories.</td>
    </tr>
    <tr>
      <td style="text-align:center">
        <p>
          <img src="https://metatron-app.github.io/metatron-doc-discovery/en/_images/Chart_Types_Icon14.png"
          alt="icon14" />
        </p>
        <p>Radar chart</p>
      </td>
      <td style="text-align:left">Cross: 1 dimension, 1 or more measures</td>
      <td style="text-align:left">Displays different quantitative variables on axes starting from the same
        point.</td>
      <td style="text-align:left">Used for a visual comparison among different quantitative variables.</td>
      <td
      style="text-align:left">Product quality evaluation in five aspects.</td>
    </tr>
    <tr>
      <td style="text-align:center">
        <p>
          <img src="https://metatron-app.github.io/metatron-doc-discovery/en/_images/Chart_Types_Icon15.png"
          alt="icon15" />
        </p>
        <p>Network diagram</p>
      </td>
      <td style="text-align:left">Subject shelf: 1 dimension / Target shelf: 1 dimension / Connecting shelf:
        1 measure</td>
      <td style="text-align:left">Diagram connecting elements in dependence relations</td>
      <td style="text-align:left">Used to view data flows regarding where data elements are generated.</td>
      <td
      style="text-align:left">Monitoring the task flows of a project</td>
    </tr>
    <tr>
      <td style="text-align:center">
        <p>
          <img src="https://metatron-app.github.io/metatron-doc-discovery/en/_images/Chart_Types_Icon16.png"
          alt="icon16" />
        </p>
        <p>Gauge chart</p>
      </td>
      <td style="text-align:left">Column: Row: 1 or more dimensions / Cross: 1 measure</td>
      <td style="text-align:left">Visualizes performance for the specified target.</td>
      <td style="text-align:left">Used to view the proportions of data elements.</td>
      <td style="text-align:left">Monitoring of profits by region</td>
    </tr>
    <tr>
      <td style="text-align:center">
        <p>
          <img src="https://metatron-app.github.io/metatron-doc-discovery/en/_images/Chart_Types_Icon17.png"
          alt="icon17" />
        </p>
        <p>Sankey diagram</p>
      </td>
      <td style="text-align:left">Column: 3 or more dimensions / Cross: 1 measure</td>
      <td style="text-align:left">Displays the proportion of each data flow by the width of the connection
        line.</td>
      <td style="text-align:left">Used to monitor data flows and their respective sizes.</td>
      <td style="text-align:left">Monitoring energy flows in a factory</td>
    </tr>
  </tbody>
</table>## Chart style attributes

Once data is pivoted, an options menu is shown on the right of the screen to allow you to set the chart style. The composition of the menu varies with chart type. This section describes the settings used universally by all chart types and the “Common Setting” items for each chart type.

### Chart style settings menu

This section describes how to configure the settings of the chart style settings menu. Note that not all the settings are shown for every chart type.

#### **Color settings**

Defines various colors used in the chart.

![](../../../.gitbook/assets/image%20%28291%29.png)

1. **Graph color setting:** Set criteria to classify data on the chart by color, and select a coloring theme.

   > * **Series:** Colors data elements differently with measures.
   > * **Dimension:** Colors data elements differently with dimensions.
   > * **Measure:** Colors elements differently with the size of each aggregate of measure values.

2. **Setting color range:** This setting is displayed when **Measure** is selected as the criterion to classify data by color. Set “ON” to set colors differently with each range of measure values. The measure data to be colored can be subdivided into as many ranges as you want, starting with the lowest one. To add a new range, adjust the upper limit of the highest range and click **Add new range**.

#### **Number format**

Defines how to display numerical text data on the chart graph. To use this function, turn on Show Axis Label in the Data Label Settings Menu.

![](../../../.gitbook/assets/image%20%28332%29.png)

1. **Format:** Select a display format for numeric values from among number, currency, percent, and exponent.
2. **Decimal place:** Set how many digits to display after the decimal point.
3. **Number abbreviations:** You can use K \(thousands\), M \(millions\), or B \(billions\) as an abbreviation for a large numeric value. Select **Automation** to automatically set the most proper symbol in accordance with the number of digits.
4. **Thousands separator:** Select whether to add thousands separators when displaying numeric data values.
5. **Customer symbol:** Insert a custom text before/after numeric data values.
6. **Preview:** Displays the result of the defined number format.

#### **Y-axis setting \(when chart type is vertical\)**

If you set the chart direction **Horizontal** in the Common Setting area, the settings are exchanged between X-axis and Y-axis.

![](../../../.gitbook/assets/image%20%28375%29.png)

1. **Show axis title:** Used to set a title for the Y-axis of the chart. Disabling this function hides the title of the Y-axis.
2. **Show axis label:** Select whether or not to show the data labels on the Y-axis of the chart. Disabling this function hides the data labels on the Y-axis.

   > * **Label setting:** Set the numeric format of the data labels on the Y-axis. Set automatic to import the settings of **Format** or manual to set specific format for the data labels on the Y-axis.

#### **X-axis setting \(when chart type is vertical\)**

Defines how to display the X-axis of the chart. If you set the chart direction **Horizontal** in the Common Setting area, the settings are exchanged between X-axis and Y-axis.

![](../../../.gitbook/assets/image%20%28464%29.png)

1. **Show axis title:** Used to set a title for the X-axis of the chart. Disabling this function hides the title of the X-axis.
2. **Show axis label:** Select whether or not to show the data labels on the X-axis of the chart. Disabling this function hides the data labels on the X-axis.

   > * **Rotation:** Select an angle for the data labels on the X-axis from among 0, 45, and 90 degrees.

#### **Data label setting**

Selects whether to display the data values on the chart graph.

![](../../../.gitbook/assets/image%20%28252%29.png)

### Common settings

This section describes how to style the six most popular chart types \(bar chart, table, line chart, scatter chart, heatmap, and pie chart\).

![](../../../.gitbook/assets/image%20%28302%29.png)

#### Bar chart

This type of chart presents data values in each category of a dimension column with rectangular bars.

![](../../../.gitbook/assets/image%20%28222%29.png)

1. **Chart type**

   > * **Vertical:** Displays data values as vertical bars with the dimension axis set vertical.
   > * **Horizontal:** Displays data values as horizontal bars with the dimension axis set horizontal.
   > * **Parallel:** If more than one measure are selected, different bars representing those measures are displayed in parallel.
   > * **Stacked:** If more than one measure are selected, different bars representing those measures are stacked at one position.

2. **Limitation:** Set how many columns to display on the chart.

#### Table

A table block is formed based on the categories into which the dimension columns on the column/row shelves are grouped; accordingly, the values of the measure columns on the cross shelf are displayed as text in the crossings.

![](../../../.gitbook/assets/image%20%28117%29.png)

1. **Chart type**

   > * **Pivot:** Aggregates \(`SUM`, `MIN`, `MAX`, etc\) measure values for each pair of column and row dimensions into a different cell.
   > * **Original:** Displays all original measure values as unaggregated together with the selected dimensions.
   > * **Vertical:** Displays measure values vertically in the table. This cannot be used when “Original” is selected for displaying the table.
   > * **Horizontal:** Displays the table horizontally when “Pivot” is selected for displaying the table. Displays measure values horizontally in the table.

2. **Show head column:** Set horizontal and vertical text alignment in the column headers. When “Original” is selected, the column headers are necessarily shown. When “Pivot” is selected, you may optionally hide the column headers.

#### Line chart

This type of chart presents data values in each category of a dimension column with points. Adjacent data points are connected with each other. This type of chart is used to view trends.

![](../../../.gitbook/assets/image%20%28381%29.png)

**Chart type**

> * **Line type:** Displays the chart graph by drawing lines between points that represent measure value aggregates.
> * **Area type:** Colors the area formed by the connecting lines.
> * **Line & point:** Shows both the data points and connecting lines.
> * **Point:** Shows the data points only.
> * **Line:** Shows the connecting lines only.
> * **Basic:** Displays each aggregate as it is on the chart.
> * **Cumulative:** Displays cumulative aggregates on the chart.

#### Scatter chart

This type of chart presents data values in each category of a dimension column with defined symbols.

![](../../../.gitbook/assets/image%20%28153%29.png)

1. **Symbol type:** Set the shape of the symbol to be shown on the chart.
2. **Symbol transparency:** Set the transparency of the symbol to be shown on the chart. You can set colors either solid or transparent.

**Heatmap**

This type of chart displays values aggregated from the measure column placed on the cross shelf by using colors. For a larger aggregated value, a darker color is applied. The heatmap type does not provide any common settings.

![](../../../.gitbook/assets/image%20%28209%29.png)

#### **Pie chart**

This type of chart visualizes the proportion of each category of the dimension column.

![](../../../.gitbook/assets/image%20%28328%29.png)

**Chart type**

> * **Sector:** Displays a pie-shaped chart.
> * **Donut:** Displays a donut-shaped chart.

## Map**s** and spatial operations

BigConnect Discovery offers a **map view** function for visualizations of location data. Creating a chart in map view involves different conditions compared to other chart types.

![](../../../.gitbook/assets/image%20%28435%29.png)

* At least one **location** dimension is required.
* Data is placed on **map layer shelves** instead of the row/column/intersection shelves.
* **Style properties** are set for each layer.
* **Spatial operations** are provided.

### Location dimensions

To use map view, dimension columns of WKT geometry types such as `Point`, `LineString`, and `Polygon` must be placed on the layer shelf. There are largely three types of location data.

* **Point:** This is a 2D coordinate geometry type comprised of x and y values. Similar to GPS data, a point has a latitude and longitude.
* **Line:** This is a geometry type with line coordinates. WKT representations of LineString and MultiLineString are supported.
* **Polygon**: This is a geometry type with shape coordinates. WKT representations of Polygon and MultiPolygon are supported.

![](../../../.gitbook/assets/image%20%28471%29.png)

### Map layer shelves

![](../../../.gitbook/assets/image%20%28321%29.png)

Map view uses map layer shelves instead of the row/column/intersection shelves that are used by other chart types. A map layer shelf requires at least one location dimension.

![](../../../.gitbook/assets/image%20%28297%29.png)

When a string dimension is placed on a map layer shelf, data points are colored based on its elements; when the mouse is over a data point, the corresponding string is displayed in the data tooltip.

![](../../../.gitbook/assets/image%20%2818%29.png)

You can also place measures on a layer shelf to classify colors by measure and simultaneously represent different point sizes based on that measure. As with dimension values, the corresponding measure is displayed in the tooltip.

![](../../../.gitbook/assets/image%20%28116%29.png)

#### Add layer shelf

Click the `+`button on the right of a layer shelf to add another layer on top of the first layer. Each layer must use a different data source, and columns of only one data source are allowed to be placed per layer. Currently, up to two layer shelves are supported.

![](../../../.gitbook/assets/image%20%28426%29.png)

### Style properties

#### Common setting

In the map layer, you can choose the type of map style that represents the base map. By utilizing OpenStreetMap, three map styles are provided by default.

![](../../../.gitbook/assets/image%20%2826%29.png)

* OpenStreetMap **Light** \(Default\)
* OpenStree Map **Dark**
* OpenStreetMap **Colored**

#### Layer settings

Sets how to express layers. When a layer shelf is added, separate setting menus are created for the first and second layers.

#### **Layer properties of point type**

![](../../../.gitbook/assets/image%20%28227%29.png)

1. **Layer Name** : Set a name of the layer for legend and tooltip settings in the map view.
2. **The Type Layer** : the Data Points the CAN be Displayed on the Map the AS Is the `Point`, `Heatmap`, `Hexagon`, or `Cluster`. The point type is selected by default.
3. **The Type Point** : With `Point`the Selected Layer the AS Is the type, the Choose the CAN you Is the the Shape of the Data Points from among `Circle`, `Square`, and `Triangle`. Circle is selected by default. The shapes are displayed on the map when cluster use is set to Off.
4. **Color** : Data points can be distinguished by color based on a string dimension or a measure on the layer shelf. A color can be picked from the palette if color standards are not available. The transparency can be set as a% value.
5. **Size** : If the layer type is `Point`, data points can be distinguished by size based on a measure on the layer shelf.
6. **Outline** : When set to On, an outline is drawn for each data point. The default is Off, and the color and thickness are customizable.
7. **Cluster Distance** : With `Cluster`selected as the layer type, you can set the cluster distance as a% value. The use of clusters is recommended to optimize browser performance when working with a large number of data points.
8. **Blur** : With `Heatmap`selected as the layer type, you can adjust the blur effect on the heat map. The default is 20%.
9. **Radius** : If the layer type is `Heatmap`or `Hexagon`, the display radius can be adjusted in the range of 1 to 100.

#### **Layer properties of line type**

![](../../../.gitbook/assets/image%20%28390%29.png)

1. **ayer Name** : Set a name of the layer for legend and tooltip settings in the map view.
2. **Color** : Data points can be distinguished by color based on a string dimension or a measure on the layer shelf. A color can be picked from the palette if color standards are not available. The transparency can be set as a% value.
3. **Thickness** : Set the line thickness.
4. **Line type** : Choose among a solid line, dotted line, and dashed line. The default is a solid line.

#### **Layer properties of polygon type**

![](../../../.gitbook/assets/image%20%28414%29.png)

1. **Layer Name** : Set a name of the layer for legend and tooltip settings in the map view.
2. **Color** : Data points can be distinguished by color based on a string dimension or a measure on the layer shelf. A color can be picked from the palette if color standards are not available. The transparency can be set as a% value.
3. **Outline** : When set to On, an outline is drawn for each polygon. The default is Off, and the color and thickness are customizable.

#### Legend settings

Choose whether or not to display a legend. The default is Off. The position of the legend can be set when turned on.

![](../../../.gitbook/assets/image%20%28380%29.png)

### Spatial analysis

The map view of BigConnect Discovery supports simple spatial analysis between two layers. Spatial operations can be set in the analysis tab on the left, and the current version supports two types of spatial operations.

![](../../../.gitbook/assets/image%20%28195%29.png)

* **Within** : This returns values ​​within a distance designated between elements of the Main and Compare Layers.
* **Intersection** : This method returns overlapping areas between the Main and Compare Layers. Return values ​​may vary with the scale of the geometry selected \(Polygon&gt; Line&gt; Point\).

Additional settings that can be customized for each operation are as follows:

* **Buffer** : Set a tolerant distance within which the Main and Compare Layers could be compared. The distance can be set either in meters or in kilometers.
* **Choropleth map** : The resulting layer can be displayed in the form of a choropleth map. The color scheme of the choropleth map can be selected; by default, colors are divided according to the data count. If the Main Layer includes a measure, colors can be changed based on its elements.

