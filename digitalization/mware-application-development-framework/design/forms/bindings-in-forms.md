# Bindings in forms

Forms are a critical part of most apps as they allow your users to collect, edit and submit data.

While bindings are not necessary to build functional forms for the internal DB and SQL tables, they do provide additional flexibility for your form fields when you need dynamic labels, placeholders, or default values.

Furthermore, you may want to read the value of a form field when handling such cases as:

* On change event
* Conditional UI
* Filtering

***

### Read Query: Setting default values

A default value binding will be applied on the initial load of a screen. This is useful when pulling values from a Singleton source.

Consider a REST query that retrieves an employee record for a bound ID. We can add a Repeater block to a Screen to pull that employee data.

**Step 1 - Setup the repeater block**

<figure><img src="https://files.readme.io/97b11fa-small-GET_employee_row.png" alt="REST query to GET a single employee from rowId binding"><figcaption><p>REST query to GET a single employee from rowId binding</p></figcaption></figure>

<figure><img src="https://files.readme.io/da569f0-small-Screenshot_2023-05-10_at_13.56.03.png" alt="Selecting the GET query as the data source"><figcaption><p>Selecting the GET query as the data source</p></figcaption></figure>

After selecting the GET query as the data source for the repeater block, click on the cog icon and provide a binding value.

In this example I am using the `{{ URL.id }}` binding which is made available through URL variables. You can however use any value here, including App state.

<figure><img src="https://files.readme.io/c632ad8-small-Screenshot_2023-05-10_at_14.00.36.png" alt="Passing the rowId into the GET query"><figcaption><p>Passing the rowId into the GET query</p></figcaption></figure>

**Step 2 - Configure the form**

Add a [Form](https://docs.budibase.com/docs/forms). Add the form fields that you would like to pull data into. Make sure to enter a unique name in the _Field_ setting, or select a field if you are also using a [Create query](https://docs.budibase.com/docs/form-bindings#create-query-mapping-query-bindings-to-form-fields).

For the _Default value_ setting, click on the lightning bolt icon, and add the binding to pull out the specific field, for example _Address_:

<figure><img src="https://files.readme.io/d64c9ea-small-Screenshot_2023-05-10_at_14.21.20.png" alt=""><figcaption></figcaption></figure>

Default value binding example

```Text
{{ Employee Repeater block.GET Employee Row.Address }}
```

Once all fields have been added with their appropriate default values, you should see something like so:

<figure><img src="https://files.readme.io/949df27-small-Screenshot_2023-05-10_at_14.22.38.png" alt=""><figcaption></figcaption></figure>

Note that you may need to refresh the page - remember that default values only apply on load of the screen.

***

### Create Query: Mapping query bindings to form fields

If you are building a create form, then it is possible to use the bindings of a 'Create' type custom query as the schema of your form. This means that the form fields will automatically be populated in a Field group to match the query inputs.

For example consider the case of a POST REST query:

**Step 1 - Add the REST query**

Add your binding fields, and optionally set some default values. These will be used in the body of your POST query, and as the schema for your form in the Design section.

<figure><img src="https://files.readme.io/1859371-bindings.png" alt="POST query bindings"><figcaption><p>POST query bindings</p></figcaption></figure>

<figure><img src="https://files.readme.io/b051c30-body.png" alt="POST query body"><figcaption><p>POST query body</p></figcaption></figure>

**Step 2 - Add the form**

Add a form and select your create query as the schema.

<figure><img src="https://files.readme.io/b3b08e7-Screenshot_2023-07-18_at_17.26.39.png" alt="Selecting the form schema"><figcaption><p>Selecting the form schema</p></figcaption></figure>

Next add a field group, and click the `Update form fields`. You can see that this will now populate the Component Tree with form fields that match your bindings.

<figure><img src="https://files.readme.io/57c3a49-Screenshot_2023-07-18_at_17.28.26.png" alt="Auto-populating form fields via the field group"><figcaption><p>Auto-populating form fields via the field group</p></figcaption></figure>

Finally you can add a submit [Button](https://docs.budibase.com/docs/button) with an [Execute query action](https://docs.budibase.com/docs/data-actions#execute-query). Bind the query bindings to your the form fields:

![](https://files.readme.io/2547c3b-Screenshot\_2023-07-18\_at\_18.07.33.png)

> ### 👍
>
> It is possible to combine the Read and Create query examples shown here into a single form.

***

### On Change event

If you want to perform Actions when a form field value has changed, you can use the _On Change_ event.

Within the Settings Panel of each form field you can find the `Define action` button beside the _On change_ label.

#### Tutorial: Calculate age from DOB field

1. Create a new app with a new internal DB table called **Patients**. Add a Text field for the _Name_ of the patient, and another Date/Time field for the _DOB_. Add some rows.

<figure><img src="https://files.readme.io/0ef4ed4-small-Screenshot_2023-05-10_at_14.49.57.png" alt=""><figcaption></figcaption></figure>

2. Add a **List view** Autogenerated screen for the **Patients** table and Eject the Table block and nested _Details_ Form block.
3. In the Field group within the _Details side panel_ form, add another field, this time a Number field, and make it _Disabled_. Type 'Age' into the _Field_ name setting.

<figure><img src="https://files.readme.io/34b7ea7-small-Screenshot_2023-05-10_at_14.59.28.png" alt=""><figcaption></figcaption></figure>

4. Next select the _DOB_ field, and click on `Define actions` under the _On change_ setting. Add an **Update Field Value** action for the _Details Form_, and choose the 'Age' field as the target.\
   For the value, click on the lightning bolt icon and select the _JavaScript_ tab. Here we will calculate the _Age_ from the _DOB_.

<figure><img src="https://files.readme.io/a52f4f6-small-Screenshot_2023-05-10_at_15.04.20.png" alt=""><figcaption></figcaption></figure>

5. Provide the following JavaScript binding:

Calculate Age from DOB

```javascript
/**
* Source: https://stackoverflow.com/questions/10008050/get-age-from-birthdate
*/
function getAge(dateString) {
    var today = new Date();
    var birthDate = new Date(dateString);
    var age = today.getFullYear() - birthDate.getFullYear();
    var m = today.getMonth() - birthDate.getMonth();
    if (m < 0 || (m === 0 && today.getDate() < birthDate.getDate())) {
        age--;
    }
    return age;
}

return getAge($("Field Value"));
```

Make sure to save! Also note the binding that is being used - we want to use the newly selected date value:

> ### 👍`{{ Field Value }}`
>
> Gets the current field value.

> ### ❗️`{{ Details Form.Fields.DOB }}`
>
> Will get the previous value; before the on change was triggered.

\
