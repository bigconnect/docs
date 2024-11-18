# Form steps

> ### Multi-step form block
>
> If you're working with a single data table, the Multi-step form block is the quickest way to get started!

There are a few different options which can be selected within the action `Change Form Step`.

* **Next Step** - Allows the user to move to the next form step. (Majority of the time this is attached to a button on click action)
* **Previous Step** - Allows the user to move to the previous form step.
* **First Step** - When used change the form step back to 1.
* **Specific Step** - Allows the user to jump between steps e.g. can go from step 1 to step 5.

<figure><img src="https://files.readme.io/f43cf1b-Screenshot_2023-11-07_at_09.21.02.png" alt=""><figcaption></figcaption></figure>

A more complex example of form stepping may be wanting to create a form where you can enter employee personal details in the first step, and employee address details in a second. Upon submission of the form, linked entries will be saved into two tables.

### Data

Create two tables with a many-many relationship:

* Employee
* Address

<figure><img src="https://files.readme.io/677379a-employee.png" alt=""><figcaption></figcaption></figure>

<figure><img src="https://files.readme.io/cd6afaa-address.png" alt=""><figcaption></figcaption></figure>

### Design

Add the default screens for the Employee table, and add a form with two form steps into the `/new/row` screen.\
The form schema should be set to Employee.

![](https://files.readme.io/9002dc5-Screenshot\_2023-07-04\_at\_13.53.27.png)

Next add fields into Step 1 that match the column names in the Employee table.\
Also add in a Next button which will be used in the form navigation.

<figure><img src="https://files.readme.io/2573ad6-Screenshot_2022-07-21_at_13.49.40.png" alt=""><figcaption></figcaption></figure>

**Define actions** for the Next button. Add the 'Change Form Step' action and set the step value to 'Next Step'.

<figure><img src="https://files.readme.io/d64e046-Screenshot_2022-04-14_at_17.13.52.png" alt=""><figcaption></figcaption></figure>

Now add fields into Step 2. This time the field names should match the columns in the Address table.\
In addition, there are now two buttons:

* Back, which can be configured as above but should naturally navigate to the 'Previous step'.
* Save, which will submit our form, saving a new linked entry into both the employee and address tables.

<figure><img src="https://files.readme.io/819b2e3-Screenshot_2022-07-21_at_13.50.38.png" alt=""><figcaption></figcaption></figure>

Finally, our Save button actions will validate the form, and navigate back to `/employee` as usual.

Add a Save Row action and move it to position 2. It will save to the Address table in the standard way.

![](https://files.readme.io/7011667-Screenshot\_2022-04-14\_at\_17.35.47.png)

For the Save Row action into the Employee table, use [Action referencing](https://docs.budibase.com/docs/chaining-actions) to connect the newly created Address row.

<figure><img src="https://files.readme.io/ed160b1-Screenshot_2022-04-14_at_17.37.37.png" alt=""><figcaption></figcaption></figure>

### Custom progress bar

Often in multi-step forms you want to give the user an indication of their progress through the form.

This tutorial will show you how to add a horizontal progress bar, expanding upon the above example. It will assume three steps have been added.\
You can easily change the colours and styles, or even add Images to get the desired look.

**Step 1**

Above your _Title Container_ add a Container with a background and border set as follows:

![](https://files.readme.io/a194063-Screenshot\_2022-06-13\_at\_16.47.06.png)

**Step 2**

Add a child container.\
Set the size and background colour. In this case a custom colour is used:

![](https://files.readme.io/c89059f-Screenshot\_2022-06-13\_at\_16.49.56.png)

**Step 3**

For the child container, select Configure conditions and enter the following:

<figure><img src="https://files.readme.io/6b276b9-Screenshot_2022-06-13_at_16.54.15.png" alt=""><figcaption></figcaption></figure>

**Step 4**

Duplicate the child container twice so that there will be a segment for each form step.\
Then update the comparison number of the conditional expressions for each to be 2 and 3 respectively.\
This will ensure that as the `Form.Current Step` increases, the progress segments will display incrementally.

![](https://files.readme.io/a04fc34-progress\_containers.png)

**Step 5**

That's now functional, but as rounded corners were used on the parent container, we need to do a little CSS tidy up to prevent clipping by the child containers.

For the first segment, edit the Custom CSS as follows: `border-radius: 0.5rem 0 0 0.5rem;`\
For the last segment, update to the following: `border-radius: 0 0.5rem 0.5rem 0;`

Also as three segments were used in this example, update one of the segments to have a width of 34%.

**Result**

<figure><img src="https://files.readme.io/a22367c-formstep_conditional.gif" alt=""><figcaption></figcaption></figure>

### Spectrum meter

If you don't need to adhere to a custom theme, you may find that the [Spectrum Meter](https://spectrum.adobe.com/page/meter/) is both stylish and more convenient.

Add an Embed component to the _Title Container_ with the following content:

HTML

```html
<div style="margin: 20px 0;">
  <div class="spectrum-ProgressBar spectrum-ProgressBar--sizeM" value="{{ Form.Current Step }}" role="progressbar" aria-valuenow="{{ Form.Current Step }}" aria-valuemin="1" aria-valuemax="3">
    <div class="spectrum-FieldLabel spectrum-FieldLabel--sizeM spectrum-ProgressBar-label">Step {{ Form.Current Step }} of 3</div>
    <div class="spectrum-FieldLabel spectrum-FieldLabel--sizeM"></div>
    <div class="spectrum-ProgressBar-track">
      <div class="spectrum-ProgressBar-fill" style="width: {{ multiply (divide Form.Current Step 3) 100 }}%"></div>
    </div>
  </div>
</div>
```

The current progress is calculated as a percentage for the width of the progress bar.

<figure><img src="https://files.readme.io/60f04dd-Screenshot_2022-06-14_at_11.41.15.png" alt="" width="375"><figcaption></figcaption></figure>

### Initial form step

You have the option to set the form step directly on the form component itself. This feature is especially valuable when you want to preserve the current form step using persisted state.

<figure><img src="https://files.readme.io/6f31e6f-Screenshot_2023-11-08_at_07.41.33.png" alt=""><figcaption></figcaption></figure>
