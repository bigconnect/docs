# External Data Connector

### Usage

<figure><img src="https://files.readme.io/106598936e032a84d3c0e12e23dfb6cc36d16d8ad0eaaccc6dc6723039aebfca-Screenshot_2024-11-07_at_10.32.16.png" alt=""><figcaption></figcaption></figure>

External Data Connector action steps can be used to run a query from the Data section, either from a Data Source Custom Query, or from a REST query.

If the query to your External Data Connector takes parameters, they can be set as part of the flow by using the bindings drawer or simply manually-typed.

<figure><img src="https://files.readme.io/e01c0a4dc7ec718bebdf72494733a340dbc04a01364da71519543c96980e55fc-Screenshot_2024-11-07_at_10.32.02.png" alt=""><figcaption></figcaption></figure>

The response from the query will then be available in bindings for the subsequent steps in your automation, for example, if your query returns an array, it will be possible to pass that array on to another Action step such as Create Row, and use the Looping feature to create a row per element in the array.

<figure><img src="https://files.readme.io/e4a300f24160f53549ad4e4b90f10e371f8f3829fa6619fb4d43ba4247fe77ec-Screenshot_2024-11-07_at_10.33.16.png" alt=""><figcaption></figcaption></figure>
