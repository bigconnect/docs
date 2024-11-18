# Data in automations

The real power in automations is the ability to pass data from automation step to the next. This can be as simple as using information about a newly saved row that the automation has triggered on or it could be the result of an external API call. Throughout the system, we have used the same syntax when we allow inputs to be based on the current state. We have built a binding editor that will help you build valid syntax.

### Using bindings

The state of an automation increases the further you move through it. This is made obvious by our binding editor as it adds all of the outputs of the previous steps onto the list of options that can be used. The values cannot and will not change throughout execution. Once they have been set they are fixed, however if you wish to combine multiple steps outputs into a single output you can do something like:

```
{{ trigger.record.salesPerson }} just made a sale to {{ trigger.record.customer }}!
```

Our binding editor will keep track of all of the outputs from previous steps but it will not have an understanding of the properties that exist as part of them. In the binding editor, you will notice that some properties such as Record will be listed as an object. These particular properties will require you to add the name of a property you wish to make use of - an example of this might be:

* You have a table that holds a list of salespeople, with a column for their email address called _EmailAddress_
* An automation is created which triggers the creation of a new salesperson
* You wish to send an email to the salesperson to welcome them to the system, in which case you would bind to the\
  **\{{ trigger.record \}}** in your send email action and add the EmailAddress property yourself, so it appears as **\{{ trigger.record.EmailAddress \}}**
