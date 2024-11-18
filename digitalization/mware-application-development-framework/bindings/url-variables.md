# URL variables

Sometimes you need to pass information between screens. This takes two main forms:

1. Link element
2. 'Navigate To' Action

A common example is opening a detailed view for a row entry.

### Navigate To

Some templates such as the _Client Contact List_ app use row buttons instead of links. Tables also allow you to define actions on row click.

In these cases, navigation is controlled by the navigation action:

<figure><img src="https://files.readme.io/39afb8b-Screenshot_2022-04-20_at_17.05.15.png" alt=""><figcaption></figcaption></figure>

Here we are passing the row id of the client into the URL path.

In the `/clients/:id` details screen we will be able to make use of this row id.\
A word prefixed with a colon within a route denotes a path variable.

The id is accessed using the `{{ URL.id }}` Binding

For example, because we only want to show the chosen client, we can add the following Filter to the clients' Data Provider

<figure><img src="https://files.readme.io/e0eff42-Screenshot_2022-04-20_at_19.01.31.png" alt=""><figcaption></figcaption></figure>

This can then be used in conjunction with a Repeater to access the fields of the filtered client:

### Multiple variables

Whilst passing a row id is the most common use case, you can pass many variables into the URL, with each being separated by a slash, e.g. `/clients/:id/:companyId/:timestamp/:customName/:etc`
