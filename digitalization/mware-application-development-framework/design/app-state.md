# App state

App state is a generic store of values that can be used to easily pass values around and persist them between navigation and page reloading.

Unlike other Binding values, App state values can be set and used anywhere in your application. Nesting is not required. To set an app state value you can use an action called `Update state` in the actions drawer. There are two types; Setting and Deleting. Set a key to be the unique identifier, this is whats used when referencing the state value elsewhere in your app. You can select an already existing one or create one by typing in a name. After this, you can enter any value into the value input field. In addition to static values, you can also use Binding values here.

<figure><img src="https://files.readme.io/09f8b06-setting_state.png" alt=""><figcaption></figcaption></figure>

### App state tutorial

The below example shows a simple way which app state can be used within a app. The overview of the example includes the following, creating a select input which will save the field value to state. From here this state value will then be used to show and hide components based on the selected value.

1.  Add a Form component\


    <figure><img src="https://files.readme.io/2b8b7ab1be9ffffea0fdb757c10d1faa91e81644348b7f38e200882f2cca056c-Screenshot_2024-09-11_at_14.13.10.png" alt=""><figcaption></figcaption></figure>
2. Add a Options picker component, then change the `Options source` from schema to custom.
3.  Click the `Define Options` button and add your options for this demo the options used are: Tables, Blocks and Images.\


    <figure><img src="https://files.readme.io/d43a3df70ebbb12a05b804b6bef931f19e29e1a3b79b808f873615702be2fd74-Screenshot_2024-09-11_at_14.14.35.png" alt=""><figcaption></figcaption></figure>
4.  After this click the `On change` button, search for the `Update state` action. Add this and then configure your state settings to something similar to the below.\


    <figure><img src="https://files.readme.io/34fc455-small-update_state.png" alt=""><figcaption></figcaption></figure>
5. After this build out your page however you want, add additional options to your select input if you choose to.
6. Setup conditionals for the various components throughout using the `Configure conditions` button within the conditions tab.

<figure><img src="https://files.readme.io/5d0532d62b0c5ab5a386b1925f29d4b24aea2ede7cb9e850c0b32a283aab469c-Screenshot_2024-09-11_at_14.13.30.png" alt=""><figcaption></figcaption></figure>

**Short Demo**

<figure><img src="https://files.readme.io/85299123ceb429d1ab47c0b317f44866c7b3bd5856b55af668dc6574eba9573f-20240911_141040.gif" alt=""><figcaption></figcaption></figure>

#### App state screen generation

For more context on app state, some components already include this feature. A good example is the Auto generate screens feature, where you can quickly see app state in action by generating screens based on preexisting data. This feature is particularly useful for generating screens with side panels or forms. Once generated, you can view these side panels and forms, and observe how state is applied to the edit form types. Additionally, if you navigate to the Table component and click the `ON ROW CLICK` button, you will see some pre-generated actions that demonstrate state handling.

<figure><img src="https://files.readme.io/822a55a-Screenshot_2023-04-21_at_11.53.35.png" alt=""><figcaption></figcaption></figure>

What this is doing is getting the clicked rows `_id` and adding it to a Form Block which is automatically set to update. This newly created state key is then added into the `Details form block Row ID` field which is what is use to save any updates made back to that specific row.

<figure><img src="https://files.readme.io/ca96089-small-Screenshot_2023-05-04_at_09.50.54.png" alt=""><figcaption></figcaption></figure>

#### App state examples

There are a range of different ways app state can be used within Mware Application Development Framework. A few examples can be seen below.

* **Multiple step forms**, you can use App state to save the `_id` of the row which you are currently creating. App state can then be used to both save your place in the form and to save information at different stages back to your database as you process through your multiple step form.
* **Form Block** As talked in the **App state demo** you can use app state to pass specific rows `_id` into forms. Whether this be a form within a Side Panel component or a form on a different page, app state can be used in both cases.

***

### Persisting values

If you wish for the values that the user has selected to persist you can select the Persist this value checkbox beneath the key/value fields. Doing so will save the value in the users browser using the localStorage API. (A gif below has been attached to showcase the difference between persisted state and regular state.)

<figure><img src="https://files.readme.io/ddbaf39e0671179f451013f559d2a5f1d6df1f6f68054f0c917ede301dcc6b3b-20240912_085805.gif" alt=""><figcaption></figcaption></figure>

***

### Deleting values

Deleting a value will delete it permanently and will also override values that are persisted. (Below is an example of deleting state.)

<figure><img src="https://files.readme.io/5d79ad7-small-Screenshot_2023-05-05_at_12.28.00.png" alt=""><figcaption></figcaption></figure>

***

### Referencing state values

Once you have created a value you are free to bind to it across your application. All values live under the **State** top-level Binding value and are visible in the bindings drawer.

For example, To get the value in the 1st image you would enter `{{ State.isPersonal }}`. The pre generated example image using the Table Block component you'd have to enter `{{ state.ID_bI8jAVBgC }}`.
