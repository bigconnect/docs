# Candlestick Chart

The Candlestick chart is a chart type often used in financial charts. You can configure both a bar and a line on the same row.

<figure><img src="https://files.readme.io/8c633da-candlestick.png" alt=""><figcaption></figcaption></figure>

There's a lot of data being displayed on a single column, so when you want to use this graph you need to have access to four different [Numeric](https://docs.budibase.com/docs/number) data-points per column. This could also be a great candidate to transform the data through [Views](https://docs.budibase.com/docs/views) when you want to do some calculation based on data points you have in your database.

### Adding a Candlestick Chart

To add the Candlestick chart, first place a Data Provider and add the chart nested to it. To add the chart, click `Chart > Candlestick Chart`

<figure><img src="https://files.readme.io/19082e1d61ded619dea05f32e9d45dc03f6abbbc1fa806a5f9283291b8d77ac9-Screenshot_2024-10-28_at_13.35.54.png" alt=""><figcaption></figcaption></figure>

### Displaying data

Now that the Candlestick chart has been added to the UI, you can display data. For this type of chart you'll need 4 types of Numeric datapoints.

These 4 data points, or `columns`, are for the following fields:

| Setting | Description                                |
| ------- | ------------------------------------------ |
| Open    | The opening for the `bar` inside the chart |
| Close   | The closing for the `bar` inside the chart |
| High    | The high for the `line` inside the chart   |
| Low     | The low for the `line` inside the chart    |

> ### 👍What determines the colour?
>
> When colour will be red or green depending on the `open` and `close` values. If `close` is a lower number than `open`, the colour will be red. If the `close` is higher than the `open` then the colour will be green. This also applies to the line in the chart, not just the bar.

The labels on the x-axis are supposed to be dates, this is a financial chart after all. So when picking the `Date column`, make sure you use a Date/time field for best results.

There aren't a whole lot of other customization options in the Settings Panel for the Candlestick chart, but of course, there are some. You can find them in the reference table below.

### Settings panel

> ### 📘Data columns
>
> There are 4 data column fields, which are explained in the `Displaying data` section above.

Besides the settings already explained, there are a few which you can still configure in the Settings Panel.

| Setting                             | Description                                                                                                    |
| ----------------------------------- | -------------------------------------------------------------------------------------------------------------- |
| Provider                            | The [Data Provider](https://docs.budibase.com/docs/data-provider) as a parent of your chart                    |
| Format                              | Will divide by thousand or million on the axis and adds a `K` or `M` to the axis respectively                  |
| <p>Y-axis Label<br>X-axis label</p> | The labels you want to display next to the Y and X-axis.                                                       |
| <p>Width<br>Height</p>              | The size you want the chart to be, supports all CSS supported sizes                                            |
| Animate                             | Will animate when the data changes or on startup. Data-change can happen through Dynamic filter's for example. |

### Tutorial

#### Getting Started

Head to [https://www.alphavantage.co/](https://www.alphavantage.co/) and get an API key

In the data tab, create a new data source using the REST API connector and build a query.

<figure><img src="https://files.readme.io/a40d26214f9d95060f4c59a4feeda9f2e99a7d4206ab9b547ffb5c9d31387352-untitled_6.png" alt=""><figcaption></figcaption></figure>

Your query should include the function, symbol, and interval parameters, as well as using your API. Click "Send" to verify a successful connection.

The response-data from the API isn't quite in the right format for Budibase to make use of, so you will need to manipulate it a little using the Transformer.

<figure><img src="https://files.readme.io/8cf247b3bbce957835e9997bd204627f51a3f8110bf761aca51ad1259cee203f-Screenshot_2024-10-28_at_14.04.17.png" alt=""><figcaption></figcaption></figure>

```
let apiResponse = data

const transformedArray = Object.keys(apiResponse["Time Series (5min)"]).map(date => {
  const data = apiResponse["Time Series (5min)"][date];
  return {
    date: date,
    open: data["1. open"],
    high: data["2. high"],
    low: data["3. low"],
    close: data["4. close"],
    volume: data["5. volume"]
  };
});

return transformedArray
```

When your transformer is set up, click "Send" to test the query. The response will be an array of objects, but the JSON window will show you an example of a single object. Be sure to save your query before navigating away.

#### Displaying the Data

In the design tab, on the screen you wish to display data on, add a Data Provider and set it to fetch information from this query. Now add a Candlestick Chart set to receive data from your Data Provider. Once you have set the sources for Date, Open, High, Low, and Close, your chart will appear.

<figure><img src="https://files.readme.io/036a4c5b27942ca10f89bf8e81a14927d80691677e7e519060ff42b42412702f-image.png" alt=""><figcaption></figcaption></figure>
