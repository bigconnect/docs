# Repeater

The repeater component is used to loop over your data. Every child of your repeater will thus be repeated for every row in the data provider that you have selected.

To get started with it, you must add it as a child of a Data provider and in the settings, select that data provider. You can then add your components and get started. An example structure can be seen below:

\


<figure><img src="https://files.readme.io/704560066119c1005343aaffae00dff97be9025fa854ffd9533fb2e8f22993fa-Screenshot_2024-10-24_at_12.06.46.png" alt=""><figcaption></figcaption></figure>

### Settings

A repeater acts like a container for each of your data rows. With this in mind, it shares many of the settings found in the [Container](https://docs.budibase.com/docs/container) component.

<figure><img src="https://files.readme.io/c6d9067e518ece2ab1e00948d1ea4c8532611d991b76d5d1d198575748ec02ae-Screenshot_2024-10-24_at_12.08.43.png" alt=""><figcaption></figcaption></figure>

There is also an option for choosing a message to display if no rows are present:

\


<figure><img src="https://files.readme.io/fb8dfee-Screenshot_2022-10-07_at_17.57.18.png" alt=""><figcaption></figcaption></figure>

\


### Example: Edit screen

One important role of the Repeater component is to facilitate edit screens. Typically within an edit screen you want to fill out some form data for a single existing record.

Let's explore this further.

<figure><img src="https://files.readme.io/934c151f0ca8b4f57797aaa003bfb88137189a6f5d6f4511b153ef141600fe08-Screenshot_2024-10-24_at_12.56.02.png" alt="Edit screen component tree"><figcaption><p>Edit screen component tree</p></figcaption></figure>

1. The Data provider has a filter that returns all rows that match the URL id.\
   See more on URL variables

<figure><img src="https://files.readme.io/68c76034b9aabeaa645669ca95375c11ff0f24c5789f18405c7c5cd466643b29-Screenshot_2024-10-24_at_12.54.52.png" alt=""><figcaption></figcaption></figure>

2. As a data provider will always return an array of results, the Repeater is added to loop over the array - in this case one entry if filtered correctly.
3. Finally the form is configured with the _Update_ form type so that the form fields are pre-populated with data from the Repeater.

<figure><img src="https://files.readme.io/4b8fde389eefb5443d5d2f4725e56389eac1c4dbd70cc56fa39284bf404ea273-Screenshot_2024-10-24_at_12.56.41.png" alt=""><figcaption></figcaption></figure>
