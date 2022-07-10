# Autocompleting Text Fields

It’s been scientifically proven that Internet users are lazy (not you, of course—other Internet users). For example, some users may not fill out your form if you make them type the entire title of their country in an employment application. This is especially true if they’re filling out the form on their mobile devices. Make users’ lives easier by configuring autocomplete on a form’s text fields.

Why not just use a select field with a data provider to guide user input? Sometimes a data provider can’t encompass all possible field entries. For example, if your data provider doesn’t include [mythical countries founded on old sea platforms](https://en.wikipedia.org/wiki/Principality\_of\_Sealand), users the Principality of Sealand can’t enter anything into the select field. Instead use a text field with autocomplete so users can begin typing their country’s name and then select it from a list when it appears. Autocomplete combines a text field (accepting any response that meets your validation criteria) and common choices to select from. It’s a win-win situation.

### Configuring Autocomplete <a href="#configuring-autocomplete" id="configuring-autocomplete"></a>

Before configuring autocomplete for your text fields, create a form and add a text field. If you want the autocomplete options to be populated by a REST data provider, configure one before creating your form. Now you’re ready to configure autocomplete for the field:

1. In the field configuration sidebar, click the _Properties_ tab.
2. Click the _Autocomplete_ switcher so it’s enabled.
3. Select a data provider or create one manually. You can set up a data provider from a REST service, or manually enter the options users should see when they start typing in the text field.
4. Save and Publish the form.

![Figure 1: You can configure a manual data provider to specify the options users can select from.](https://liferay-support.zendesk.com/hc/article\_attachments/360030194272/forms-autocomplete-manually.png)

Once users begin entering text into the field, a selection list of options appears. As they enter additional text, the list is refined to include only options that contain the currently entered text. For example, the imaginary users from Sealand (all two of them) begin reluctantly typing their country of origin by entering an _S_. They’re delighted to see a selection list with a bunch of countries containing the letter _S_ appear for their selection convenience. If they continue typing and enter _e_, the list is refined to options that have _se_ in their name (for example Serbia and Senegal). If they continue typing and enter _a_, they’ll now only see one option, Sealand, if it’s in your data provider. Selecting it from the list after typing the first three letters is much easier than typing the remaining letters.

![Figure 2: When typing in a field with autocomplete, users are presented a list of selections from the configured data provider. The displayed results are filtered to include only selections containing the text entered by the user.](https://liferay-support.zendesk.com/hc/article\_attachments/360030194292/forms-autocomplete-filtering.png)

What will the Forms team think of next? Configuring telepathic connections to the Forms application would be nice. Then users could just think their form field entries into existence. Stay tuned.
