# Data

The data section in the Mware Application Development Framework UI is where you add and manage your data. Mware Application Development Framework apps rely on data and it is strongly advised to create your data structure before designing your web application. Your data structure will involve tables, columns, rows, and views - these are the building blocks that hold and present your data in a structured manner.

Predefining the data structure also greatly helps in building the UI as the UI can be automatically generated based on the data structure, as well as any logic, filtering, and sorting.

Within Mware Application Development Framework we have a single built-in Datasource and several external datasources. When using a self-hosted instance, an external datasource can still live on the same device, just not within the Mware Application Development Framework container.

### Getting started with data

By default, every application will start with a Mware Application Development Framework DB datasource. This database contains a Users table which is considered a special table that cannot be removed. This table includes all the users who have access to your application, with their respective roles.

### Adding new datasources

There are a broad set of available external datasources you can connect to right from the Mware Application Development Framework UI.

You can add new data sources by clicking the `+` button on the left-hand panel when you're on the Data tab, as seen in the image below.

<figure><img src="https://files.readme.io/1f54f3e-add_ds.png" alt="Adding a datasource"><figcaption><p>Adding a datasource</p></figcaption></figure>

Once you've clicked the `Add source` button, you'll see a modal presented with the available datasources. Pick your datasource of choice, and you will be presented with a configuration screen to connect to your remote datasource.

<figure><img src="https://files.readme.io/8ff4475-new_ds.png" alt="Choose a datasource"><figcaption><p>Choose a datasource</p></figcaption></figure>

You can add as many datasources as you need following the same process.

To know more about a particular datasource, look at the relevant documentation page in the menu.
