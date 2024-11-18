# Data in design

A Data provider fetches data and provides it to all child components. Any child components which take in data to display, have a _Data_ setting where you can select the data provider that passes it data. You can hook up as many components as you like to the same data provider, and they'll all be in sync with each other.

Data providers have settings to control filtering, sorting, and pagination. Because all of these data settings are handled by the data provider, it keeps all components as purely presentational. You can update the source of your data provider and your whole dashboard will instantly reflect an entirely different table!

<figure><img src="https://files.readme.io/ce1377f3dd99ef71ab01118c3ead27ff2b543ea392da319397fc1e320f605036-Screen_Recording_2024-09-25_at_13.31.07.gif" alt=""><figcaption></figcaption></figure>

> ### 👍Blocks
>
> For common data presentations, you can use Blocks.
>
> Blocks bundle a data provider with other components, thus reducing the nesting in your Component Tree.

### Fetching data

After adding a Data provider, you must navigate to the Settings Panel and select a Datasource - as shown below:

<figure><img src="https://files.readme.io/79c95cf1b3e11ae7b1917002b90f3135fea2614f9cc67330c8057abcbba4cbc6-image.png" alt=""><figcaption></figcaption></figure>

> ### 🚧Using Multiple Data Providers
>
> Data Providers can use other Data Providers as a source
>
> Each time a Data Provider is set to fetch information from a table or query, it creates extra network traffic on screen-load. If you're re-using data in multiple components on your screen, consider using a Data Provider to fetch the table without filters, and then any subsequent Data Providers can fetch from that first Data Provider and filter as needed, cutting down on the amount of queries to your database.
