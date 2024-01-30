# Action: Autofill

Autofill rules let you change the selection options of another field based on the value entered into a related field. A data provider’s output is used to populate a field, as long as the condition is met.

Before configuring an autofill rule, set up a data provider. That’s how autofilled fields are populated. Pay careful attention to the input and output parameters you choose when setting up the rule.

To follow this example:

1.  Set up a data provider using the `get-countries` JSON web service. If you’re running the portal at `localhost:8080`, you can access this web service here:

    ```
    http://localhost:8080/api/jsonws?contextName=&signature=%2Fcountry%2Fget-countries-0
    ```

    Make sure the output parameter is set to `$..nameCurentValue`.
2. On the last form page, add two fields:
   * A Single Selection field called _If I win I’d like my award to be:_, with two choices: _Cash_ and _All Expenses Paid Vacation_.
   * A Select from List field called _Choose a Destination Country_. Under _Create List_, choose _From Autofill_.

**Example:** Before submitting the race registration, let users decide whether they want a cash prize or an all-expenses-paid vacation. If they choose the vacation, populate the Choose a Destination Country with output from the data provider.

To configure an Autofill rule,

1. Open the Rules tab of the Edit Form page and click the Add (![Add](https://liferay-support.zendesk.com/hc/article\_attachments/360030193992/icon-add.png)) button.
2.  Define the rule:

    * If field _If I win I’d like my award to be_ is equal to the Value _All Expenses Paid Vacation_, Autofill the _Choose a Destination Country_ field from the _countries_ data provider (note that you might have named this differently when setting it up).

    <img src="https://liferay-support.zendesk.com/hc/article_attachments/360030162651/forms-autofill.png" alt="Figure 1: Build form rules quickly by defining your conditions and actions." data-size="original">

    Figure 1: Build form rules quickly by defining your conditions and actions.

    * Save the rule.

    <img src="https://liferay-support.zendesk.com/hc/article_attachments/360030162611/forms-autofill2.png" alt="Figure 2: Once a rule is saved, it is displayed so that you can easily understand what it does." data-size="original">

    Figure 2: Once a rule is saved, it is displayed so that you can easily understand what it does.

### Using Inputs with Autofill <a href="#using-inputs-with-autofill" id="using-inputs-with-autofill"></a>

The above example is simple, using only an Output to autofill a Select from List field if another field has a certain value. Many times, the response from the REST provider must be filtered before display in the Select from List field. For this, a Data Provider Input field is required. For example, to configure an autofill rule to display the countries of the world filtered by a Region field (for example, Americas, Europe, or Oceania),

1.  Create a Data Provider.

    **Name:** `restcountries`

    **URL:** `https://restcountries.eu/rest/v2/region/{region}?fields=name`

    **Inputs:** Fill in the Label (_region_), Parameter (_region_), and Type (_Text_).

    **Outputs:** fill out a Label (_name_), Path (_$..name_), and Type (_List_).

    <img src="https://liferay-support.zendesk.com/hc/article_attachments/360030162631/forms-autofill-data-provider.png" alt="Figure 3: Create a data provider for the autofill rule." data-size="original">

    Figure 3: Create a data provider for the autofill rule.
2.  Create a form with these fields:

    **Text:** Use the Label _Region_.

    **Select from List:** Label it _Country_, and choose _From Autofill_ under Create List.

    <img src="https://liferay-support.zendesk.com/hc/article_attachments/360030193952/forms-autofill-input-output-fields.png" alt="Figure 4: Create a form with a text field and a select from list field. These are used to provide the input to the data provider and be autofilled by its output." data-size="original">

    Figure 4: Create a form with a text field and a select from list field. These are used to provide the input to the data provider and be autofilled by its output.
3.  Configure the Autofill rule.

    **Condition:** If _Region_ **Is not Empty**

    **Action:** Do **Autofill** From Data Provider `restcountries`, Data Provider’s Input: region—_Region_, Data Provider’s Output: name—_Country_.

    <img src="https://liferay-support.zendesk.com/hc/article_attachments/360030194032/forms-autofill-rule.png" alt="Figure 5: Create the autofill rule. Brag of your prowess." data-size="original">

    Figure 5: Create the autofill rule. Brag of your prowess.

Once you’re done, publish the form and try it out, by entering a valid region into the Region field, and observing that the options in the Select from List Field are filtered based on the Region. The [restcountries.eu](https://restcountries.eu/) service has these regions you can use: Africa, Americas, Asia, Europe, Oceania, and Polar.

![Figure 6: Filter countries by region of the world.](https://liferay-support.zendesk.com/hc/article\_attachments/360030193972/forms-autofill-region.gif)

Autofill rules combine the power of data providers and form rules.
