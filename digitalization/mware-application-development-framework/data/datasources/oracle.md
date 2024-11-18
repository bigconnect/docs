# Oracle

Connecting Mware Application Development Framework to an existing Oracle database

> ### 👍SQL Databases
>
> Mware Application Development Framework is designed to integrate with any core SQL database.

## Tutorial: Building a Departments Portal

In this tutorial we will be building an app to allow executive staff to view and manage department and employee details.

To get started, follow the instructions from the Database Star sample database repo, running the setup scripts against your Oracle instance.

### Adding the datasource

With your Oracle instance running, and the sample database installed, we can begin pulling that data into Mware Application Development Framework.

First create an app - no sample data needed.

<figure><img src="https://files.readme.io/e745f7b-oracle-create-your-app.png" alt=""><figcaption></figcaption></figure>

Once the new app has been created, click `Add source` and select `Oracle`.

<figure><img src="https://files.readme.io/a819d75-1_oracle.png" alt=""><figcaption></figcaption></figure>

Click `Continue` and then fill out the connection config with the host, service name and user of your Oracle instance.

<figure><img src="https://files.readme.io/129f53b-Screenshot_2023-03-10_at_10.25.26.png" alt=""><figcaption></figcaption></figure>

Click `Save and fetch tables` to finish.



### Defining relationships

With our tables fetched, we can now define relationships within Mware Application Development Framework.

Click the `Define relationship` button below the **Table** section to get started, and setup the following relationships:

* One REGION -> Many COUNTRIES
* One COUNTRY -> Many LOCATIONS
* One LOCATION -> Many DEPARTMENTS
* One DEPARTMENT -> Many EMPLOYEES

<figure><img src="https://files.readme.io/c563ce4-Screenshot_2023-03-10_at_10.39.53.png" alt="Example relationship: One Country -> Many Locations"><figcaption><p>Example relationship: One Country -> Many Locations</p></figcaption></figure>

Once finished you should see a list of your defined relationships:

<figure><img src="https://files.readme.io/4c0801f-Screenshot_2023-03-10_at_10.40.41.png" alt=""><figcaption></figcaption></figure>

It is also worth changing the display column on some of the tables so that the links are more human readable. For example edit the **REGION\_NAME** column of the **REGIONS** table and toggle on `Use as table display column`.

<figure><img src="https://files.readme.io/16c9a9d-Screenshot_2023-03-10_at_10.43.20.png" alt="Setting the region name as the display column"><figcaption><p>Setting the region name as the display column</p></figcaption></figure>

<figure><img src="https://files.readme.io/c0d00f9-Screenshot_2023-03-10_at_10.44.20.png" alt="Linked region in the countries table is now human readable"><figcaption><p>Linked region in the countries table is now human readable</p></figcaption></figure>

### Region map screen

Our **REGION** table includes four regions. To start of with, let's present the user with a world map from which they can select a region and further drill down into country > location > department > employees.

Click on the **Design** tab along the top. Next add the Background image component, with a URL to a World Map image. Set the _Height_ and _Width_ to `100%`.

Next add a Section and set the type to `Three columns` and Custom CSS to set the height:

CSS

```css
height: 100%;
```

Next add three Containers. Within each container add a [Headline](https://docs.budibase.com/docs/displaying-text#headline) for each of the Regions: _Americas_, _Europe_, _Middle East and Africa_, and _Asia_. Add margin and adjust the container layouts as desired.

<figure><img src="https://files.readme.io/fc68bd8-Screenshot_2023-03-29_at_09.49.42.png" alt=""><figcaption></figcaption></figure>

\


### Country screen

With our region map setup, we can add a country screen that will list the countries of the selected region.

Click `Add screen` and select `List view`.

<figure><img src="https://files.readme.io/6b645bb-Screenshot_2023-03-27_at_14.26.27.png" alt="Adding an auto-generated screen"><figcaption><p>Adding an auto-generated screen</p></figcaption></figure>

Click `Continue`, select the **COUNTRIES** table, and then `Confirm`.

<figure><img src="https://files.readme.io/ceceb7e-Screenshot_2023-03-27_at_14.28.55.png" alt=""><figcaption></figcaption></figure>

You will now see your new screen with a pre-populated countries Table block. Click `Configure columns` in the Settings Panel, `Add all columns`, and then click the `X` to remove the ID columns and the _REGIONS_.

<figure><img src="https://files.readme.io/32dd26a-Screenshot_2023-03-27_at_14.34.18.png" alt="Configuring table columns"><figcaption><p>Configuring table columns</p></figcaption></figure>

You can see the locations are showing an ID field value, which isn't ideal. We can improve this by changing the display column. Navigate to the **LOCATIONS** in the **Data** section and set the _City_ column to the display column.

We can further improve the user experience by adding country flag images to each row, and allow users to search the columns.

Untick the setting `Show button above table`. All the countries have already been added, and we don't want the user to be able to enter new records.

#### Search fields

Adding searchable fields is straightforward. Within the settings panel, select the _COUNTRY\_NAME_ and _LOCATIONS.CITY_ options under the **Search Fields** property.

<figure><img src="https://files.readme.io/c6069b5-Screenshot_2023-03-27_at_14.45.27.png" alt="Selecting search fields"><figcaption><p>Selecting search fields</p></figcaption></figure>

The search fields will allow users to performa a "Starts with" search to find countries by name or city.

#### Row flag icons

As a nice visual cue, we can also add Image components to each row of our table. To do this we must first Eject the table block. Next select the Table component, and click the `Add component` button.

Select the image component, and set the _Width_ and _Height_ to `64px` and `36px`.

Finally, for the _URL_ property, click the lightning icon to provide the following JavaScript Binding:

JavaScript

```javascript
const country = $("Table.COUNTRIES.COUNTRY_NAME")?.toLowerCase()?.replaceAll(" ", "-");

if (country === "united-states-of-america") {
	return "https://www.rankflags.com/wp-content/uploads/2015/04/National-Flag-Of-United-States.jpg"
}

if (country === "israel") {
	return "https://upload.wikimedia.org/wikipedia/commons/d/d4/Flag_of_Israel.svg"
}

return `https://images.mapsofworld.com/${country}/${country}-flag.gif`
```

<figure><img src="https://files.readme.io/a16da66-Screenshot_2023-03-28_at_15.46.26.png" alt=""><figcaption></figcaption></figure>

#### Filter out countries with no locations

We don't want to display countries if they have no departments. Click on the [Data provider](https://docs.budibase.com/docs/data-provider) and `Define filters`. Add another filter to check if the locations relationship if empty.

<figure><img src="https://files.readme.io/c17cb30-Screenshot_2023-03-29_at_11.36.42.png" alt="Don&#x27;t retrieve locations with no city"><figcaption><p>Don't retrieve locations with no city</p></figcaption></figure>

\


### Linking the region to the country screen

While the countries screen is setup, we still need to filter the countries by region and link the screen to the chosen region.

First click on the _Screens_ icon button on the left hand panel. In the _Settings Panel_ on the right, edit the route to include a [URL variable](https://docs.budibase.com/docs/url-parameters) for the **region\_name**.

<figure><img src="https://files.readme.io/02576e6-Screenshot_2023-03-28_at_15.58.29.png" alt="Adding a region id to the countries screen"><figcaption><p>Adding a region name to the countries screen</p></figcaption></figure>

Next in the components view, click on the [Data provider](https://docs.budibase.com/docs/data-provider) and `Define filters`. Click `Add filter`, selecting _REGION\_ID_ equals binding: `{{ URL.region_name }}`.

<figure><img src="https://files.readme.io/74d6429-Screenshot_2023-03-28_at_16.09.02.png" alt="Adding a filter on the Region ID"><figcaption><p>Adding a filter on the Region ID</p></figcaption></figure>

Next navigate to the `/home` screen. For each of the headline containers, click `Define actions` for the _On Click_ property.

Add the **Navigate To** action, and select the countries screen. Then replace `:region_id` with the particular ID (1-4) for the selected region.

<figure><img src="https://files.readme.io/6932440-Screenshot_2023-03-29_at_09.42.08.png" alt="Navigate to the Region ID"><figcaption><p>Navigate to the country screen for Region 2 (Americas)</p></figcaption></figure>

#### Adding the region name to the country screen

Above the 'COUNTRIES' heading, add a Repeater block component, and nest the headline under it. Select the **REGIONS** table as the data source, and `Define filters`. Match the region ID against the ID passed in the URL. We can use the _default_ handlebars helper to provide a title while in the builder.

<figure><img src="https://files.readme.io/03529ea-Screenshot_2023-03-29_at_10.46.20.png" alt="Filter on the Region ID from the URL, or default to 1"><figcaption><p>Filter on the Region ID from the URL, or default to 1</p></figcaption></figure>

For the heading value, select the _REGION\_NAME_ binding from the dropdown:

<figure><img src="https://files.readme.io/d48887e-Screenshot_2023-03-29_at_10.48.01.png" alt=""><figcaption></figcaption></figure>

### Displaying a list of departments

The next level of data to drill into is the list of departments for the selected country.

In the `/countries/:region_id` screen, delete the **Details form block**. The countries should be static and not editable by the user - we only want to display the list of departments here.

Now in the empty **Details side panel**, add a Repeater block, and nest a Headline component. The Repeater block should have **COUNTRIES** set as the data source, with a filter on the **\_id** column against the _State ID_. This state binding represents the ID of the clicked row.

<figure><img src="https://files.readme.io/b1facbe-Screenshot_2023-03-29_at_10.59.23.png" alt=""><figcaption></figcaption></figure>

> ### 🚧Filtering on '\_id'
>
> The **\_id** field represents a URL-safe encoded id string.\
> As relationship links in Mware Application Development Framework are stored in this format, this field must be used when filtering on side panel state IDs.

The headline value should bind to the _COUNTRY\_NAME_ of the repeater block:

<figure><img src="https://files.readme.io/3b896b1-Screenshot_2023-03-29_at_11.03.13.png" alt="Selecting the country name from the repeater block"><figcaption><p>Selecting the country name from the repeater block</p></figcaption></figure>

Below the country name headline, add a [Divider](https://docs.budibase.com/docs/divider) as well as another Repeater block. This will be for the **LOCATIONS** pulled from the relationship of the **COUNTRIES** datasource.

<figure><img src="https://files.readme.io/9c95bd4-Screenshot_2023-03-29_at_11.19.39.png" alt="Selecting locations from the countries relationship"><figcaption><p>Selecting locations from the countries relationship</p></figcaption></figure>

Now within the Locations Repeater block, nest a [Cards block](https://docs.budibase.com/docs/blocks#cards-block) for the **DEPARTMENTS** relationship:

<figure><img src="https://files.readme.io/50fe8f4-Screenshot_2023-03-29_at_12.24.52.png" alt="Selecting departments from the locations relationship"><figcaption><p>Selecting departments from the locations relationship</p></figcaption></figure>

The _Title_ of the Cards block should bind to the \_City\_of the location, which can be selected from the drop-down.

Scroll further down the right hand settings panel to get to the _Cards_ section. Remove the _Subtitle_ and _Description_, but add the following binding for the individual card _Title_: `{{ Departments Cards block.DEPARTMENTS.DEPARTMENT_NAME }}`. This can be selected from the dropdown.

<figure><img src="https://files.readme.io/257dc2c-Screenshot_2023-03-29_at_12.38.41.png" alt="Final Details side panel component tree"><figcaption><p>Final Details side panel component tree</p></figcaption></figure>

#### Hiding locations that have no departments

Currently if a location has no departments, we will see the _No rows found_ message.

<figure><img src="https://files.readme.io/68bbddd-Screenshot_2023-03-29_at_12.40.31.png" alt="No rows found because Stretford has no departments"><figcaption><p>No rows found because Stretford has no departments</p></figcaption></figure>

Ideally we should not display Stretford at all in this case. Click on the _Departments Cards block_ and click on `Configure conditions`. Add a [Condition](https://docs.budibase.com/docs/conditions) to hide the component if **LOCATIONS.DEPARTMENTS** is empty.

<figure><img src="https://files.readme.io/4d4bf12-Screenshot_2023-03-29_at_12.43.43.png" alt=""><figcaption></figcaption></figure>

**Result**

<figure><img src="https://files.readme.io/c4592c1-departments.gif" alt=""><figcaption></figcaption></figure>

\


### Employees screen

The last screen we want is for displaying all the employees of a selected department. Click `Add screen` and add a `List view` screen for the **EMPLOYEES** table.

Change the _Route_ to `/employees/:department_id`. Then in the components view, click on the Table block and `Define filters`. Add a filter to match `{{ URL.department_id }}` against `DEPARTMENT.DEPARTMENT_ID`.

You can also eject the table block so that you can deselect some of the employee form fields, such as the ID and the department fields.

Back to the `/countries` screen, click on the _Departments Cards block_. Find the setting called `Link card title` and tick the checkbox. Under _Link screen_ select the `/employees/:department_id` screen. There is no need to replace the **:department\_id** in this instance, as Mware Application Development Framework knows to replace this with the **\_id** of the repeater.

<figure><img src="https://files.readme.io/993856c-Screenshot_2023-03-31_at_10.42.26.png" alt="Linking the card title to the employees screen"><figcaption><p>Linking the card title to the employees screen</p></figcaption></figure>

\


### Removing navigation links

As both the employees screen and the countries screen require an ID in the URL, we do not want to show the nav link for these screens.

Click the Configure links button and click the `X` to remove the link for employees and countries, and click `Save`.

<figure><img src="https://files.readme.io/2cfaa9e-Screenshot_2023-03-31_at_10.55.16.png" alt="Only display the home screen link"><figcaption><p>Only display the home screen link</p></figcaption></figure>



### Managing departments

Currently the departments listed for a location are read-only. It may be the case that you want _Admin_ users to be able to add new or update existing departments.

Create a new auto-generated List view screen for the **DEPARTMENTS** table, but in this case select _Admin_ access level.

<figure><img src="https://files.readme.io/c63e7f5-Screenshot_2023-03-31_at_10.58.06.png" alt="Only Admin users can view this autogenerated screen"><figcaption><p>Only Admin users can view this autogenerated screen</p></figcaption></figure>

You will also want to make sure that the navigation link for the department screens only displays for admin users.

Click on the Navigation icon on the left hand pane, and `Configure links`. Beside the `/departments` link, click the dropdown and select _Admin_, then click `Save`.

<figure><img src="https://files.readme.io/f4e0ac4-Screenshot_2023-03-31_at_11.09.59.png" alt="The departments link will be visible to admin users only - default is basic"><figcaption><p>The departments link will be visible to admin users only - default is basic</p></figcaption></figure>

To verify the permissions, click `Preview` in the upper right hand corner of the **Design** view. You can toggle the user permissions in the blue bar along the top to test.

<figure><img src="https://files.readme.io/6d7baf0-Screenshot_2023-03-31_at_11.12.21.png" alt="Verifying that admin users can see the Departments link"><figcaption><p>Verifying that admin users can see the Departments link</p></figcaption></figure>

<figure><img src="https://files.readme.io/47af628-Screenshot_2023-03-31_at_11.13.18.png" alt="Verifying that power users (and below) cannot view Departments"><figcaption><p>Verifying that power users (and below) cannot view Departments</p></figcaption></figure>
