# Tag

The `Tag` component can quickly display tags on, for example, a detail page. An optional delete button can be added, which can be configured using Actions.

![](https://files.readme.io/a18bea4-tags.png)

### Customising the tag

To customise the tag, you have several settings in the Settings Panel you can use.

| Setting              | Description                                                                                                                                                        |
| -------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Size                 | The size of text, choose between small, medium or large.                                                                                                           |
| Color                | Choose the color of the button, the text color will change accordingly.                                                                                            |
| Show Delete Icon     | Will add a `X` to the tag which is clickable                                                                                                                       |
| On click delete icon | This setting will appear when the `Show Delete Icon` setting is checked. This will allow you to configure what happens when the user clicks the `X` using Actions. |

\


### Tutorial: Movie genre tags

In this tutorial we will look at how you can use the tag component to display movie genres.

#### Setup

To get started, add an internal DB table called _Movies_, with 'Title', 'Year', and 'Image Url' columns. Add some rows.

<figure><img src="https://files.readme.io/6bab97ff8578be267cd2d669673ef4b236530403a46134c3770f721bc1109cce-Screenshot_2024-09-30_at_12.52.20.png" alt=""><figcaption></figcaption></figure>

Next go to the _Design_ section, and add a Cards block. Select the _Movies_ table as the Datasource, and bind the _Title_, _Subtitle_ and _Image URL_ values to the table columns. For example, the _Subtitle_: `{{ New Cards block.Movies.Year }}`

Your screen should look roughly like so:

<figure><img src="https://files.readme.io/75582fc5575ede8ca0b40580139ff9577e6f717de05d31edc7ad683f097fef41-Screenshot_2024-09-30_at_13.00.01.png" alt=""><figcaption></figcaption></figure>

#### Adding the tags

Go back to the _Data_ section, and add another table called _Tags_. Give it a text column called 'Name' and a Many->Many Relationship column linking to _Movies_.

<figure><img src="https://files.readme.io/b6650f8-Screenshot_2023-01-27_at_14.03.53.png" alt="Many Tags to Many Movies"><figcaption><p>Many Tags to Many Movies</p></figcaption></figure>

Add some tag rows, linking them to some movies.

![](https://files.readme.io/b77297e4a7d709e9acd23a5ce0698a666e29d2c3e8781cefec22911e3ad11e5f-Screenshot\_2024-09-30\_at\_12.52.13.png)

Head back over to the _Design_ section. To be able to add our tags, we must Eject the cards block.

<figure><img src="https://files.readme.io/2f663af-Screenshot_2023-01-27_at_14.58.13.png" alt="Component tree after ejecting the cards block"><figcaption><p>Component tree after ejecting the cards block</p></figcaption></figure>

Next add a Container nested under the _Repeater_. Set the _Gap_ to 'None'. Drag the _Spectrumcard_ into the new container.

Below the _Spectrumcard_ insert a Repeater block. Give it the name 'Tags Repeater Block', and select the _Tags_ table from the relationship.

<figure><img src="https://files.readme.io/9591886-Screenshot_2023-01-27_at_15.11.45.png" alt="Selecting the tags from relationship"><figcaption><p>Selecting the tags from relationship</p></figcaption></figure>

Finally add the Tag component underneath the _Tags Repeater Block_. Click _Align right_ on the repeaters block to align the tags to the right-hand side of the card, and choose _Row layout_.

Give the tag the text binding: `{{ Tags Repeater block.Tags.Name }}`. This can be selected from the bindings dropdown.

<figure><img src="https://files.readme.io/437c4fb-Screenshot_2023-01-27_at_15.14.29.png" alt="Aligning the tags"><figcaption><p>Aligning the tags</p></figcaption></figure>

As a finishing touch, it would be nice to give the different tags a color. To do so, we will use [Conditional UI](https://docs.budibase.com/docs/conditions). Click on `Configure conditions`: Update Setting 'Color' TO ... IF `{{ Tags Repeater block.Tags.Name }}` Equals ...

The duplicate condition button will be very useful in this case!

<figure><img src="https://files.readme.io/1a8431b0ef35f27c45a6bd467dba9cbe5e08218a620930c83c256528bdad3f12-Screenshot_2024-09-30_at_13.06.18.png" alt=""><figcaption></figcaption></figure>

**Result**

<figure><img src="https://files.readme.io/29cfa94673e1a43e953990df192770ddd9d6c71069b74d5c82b022206d03465d-Screenshot_2024-09-30_at_13.06.45.png" alt=""><figcaption></figcaption></figure>

To see how tags can be used in a SQL database, have a look at this MS SQL Server tutorial.

#### Removing tags

You may optionally also want the ability to remove tags. To do so, first check the `Show delete icon` setting. This will allow you to specify some actions.

<figure><img src="https://files.readme.io/95391e40fb89f8b8c7efb7d83561e7e041ab3dd2b9086197a2b58b1894894f02-Screenshot_2024-09-30_at_13.07.17.png" alt="Add some delete actions"><figcaption><p>Add some delete actions</p></figcaption></figure>

Add a **Save Row** action with the _Movies_ Repeater as the datasource, (not the tags repeater block), and select the _Movies_ table as the save destination.

Check _Do not display default notification_ so that a toast is not displayed. Also `Add column` for the _Tags_ relationship and give it the following JavaScript binding:

JavaScript

```javascript
return $("Repeater.Movies.Tags")?.filter(tag => tag._id !== $("Tags Repeater block.Tags._id"));
```

<figure><img src="https://files.readme.io/cba9ab6-Screenshot_2023-01-27_at_15.55.15.png" alt=""><figcaption></figcaption></figure>

The JavaScript binding may appear a little daunting to some, but to explain in pseudo-code:

> Save all of the movie tags excluding the one we are removing

After removing all tags, you will see a _No rows found_ message, which isn't what we'd like in this case.

<figure><img src="https://files.readme.io/5e55f11-Screenshot_2023-02-01_at_09.24.42.png" alt="No rows found - Tags repeater block"><figcaption><p>No rows found - Tags repeater block</p></figcaption></figure>

Ideally this should just be blank. Click on the _Tags Repeater block_ and `Configure conditions`: Hide component IF `{{ Repeater.Movies.Tags }}` Is empty.

As a finishing touch, it would be nice to add some margin when there are no tags so that the vertical spacing is consistent. Click on the _Spectrumcard_ and `Configure conditions`: Update Setting 'Custom CSS' TO `margin-bottom: 32px;` IF `{{ Repeater.Movies.Tags }}` Is empty.

**Naive result**

<figure><img src="https://files.readme.io/c4aab030b7a379ec65fc3cc34a803490e6d6f89102a30f9caba3487c8b171aec-20240930_131332.gif" alt=""><figcaption></figcaption></figure>

As you probably noticed, there is substantial delay between clicking the 'x' and the tag being removed. Ideally in this scenario we don't want to await for a database response. We can use [App state](https://docs.budibase.com/docs/app-state) to cache the tags and improve the tag deletion UX.

Add a second action for updating app state:

<figure><img src="https://files.readme.io/9582cd7-Screenshot_2023-01-27_at_16.21.03.png" alt=""><figcaption></figcaption></figure>

JavaScript

```javascript
return {
	...$("State.RemovedTags"),
	[$("Tags Repeater block.Tags._id")+$("Repeater.Movies._id")]: true
}
```

Next we want to replace the **Save Row** action with a **Trigger Automation** action instead. Check _Create a new automation_, enter a name and check _Do not display default notification_.

`Add Field` for the _tags_, and _movie\_id_.

![](https://files.readme.io/8e7b53b-Screenshot\_2023-01-27\_at\_16.58.10.png)

The JavaScript binding for the _tags_ field looks as follows:

tags

```javascript
return $("Repeater.Movies.Tags")?.filter(tag => !$("State.RemovedTags")?.[tag._id+$("Repeater.Movies._id")]);
```

Here we are only looking to save tags for each movie that have not been flagged as removed in our front-end cache.

Instead of waiting for a save row response, we will fire and forget a request to our Automation which will remove the chosen tag from our chosen movie.

After saving those actions, navigate to the _Automate_ section and select the **DeleteTag** automation that was automatically created for us.

Make sure _tags_ is set to type **Array**.

<figure><img src="https://files.readme.io/770eb71-Screenshot_2023-01-27_at_17.00.05.png" alt="Set tags to type Array"><figcaption><p>Set tags to type Array</p></figcaption></figure>

Next add an **Update Row** step for the _Movies_ table as follows, making sure the `Use bindings` options is selected!

In addition, check the `Clear relationships if empty?` flag to ensure that all tags can be removed.

<figure><img src="https://files.readme.io/1b46ad8-Screenshot_2023-02-01_at_10.15.08.png" alt=""><figcaption></figcaption></figure>

> ### 👍
>
> Publish your app to activate the automation.

Navigating back to the _Design_ section, add another condition to the tag: Hide component IF

JavaScript

```javascript
return $("State.RemovedTags")?.[$("Tags Repeater block.Tags._id")+$("Repeater.Movies._id")];
```

Equals Boolean True.

<figure><img src="https://files.readme.io/f7e415e-Screenshot_2023-01-27_at_16.35.28.png" alt=""><figcaption></figcaption></figure>

**Faster result**

<figure><img src="https://files.readme.io/f4741db-fast_close.gif" alt=""><figcaption></figcaption></figure>
