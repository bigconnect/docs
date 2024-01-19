# Actions on dashboards

<figure><img src="https://www.metabase.com/docs/latest/dashboards/images/dashboard-filter-action.png" alt=""><figcaption></figcaption></figure>

To put actions in action, you can combine them with a model and a filter on a dashboard.

## Adding an action to a dashboard

Visit a dashboard page and click on the **pencil** icon, then click on the **box with a mouse pointer** to add an action.

Answers will add an action button to the dashboard grid, and open a sidebar with the buttons settings.

Button text

A label explaining what the button does, e.g., “Ignition”.

Button variant

You can select from a variety of handsome buttons:

* Primary
* Outline
* Danger
* Success
* Borderless

<figure><img src="https://www.metabase.com/docs/latest/dashboards/images/buttons.png" alt=""><figcaption></figcaption></figure>

## Connecting an action to a dashboard filter

For most actions, when people click on a button, they’ll be prompted to input values in the fields defined by that action.

Many types of actions rely on knowing the entity IDs for the model to determine which records to update or delete. To have people select the ID for the action, you’ll need to:

1. Click on the pencil icon to edit the dashboard.
2. Add an action to the dashboard (if you haven’t already).
3. Add a filter to a dashboard, wire it up to any cards that you want to, and click **Done** in the bottom of the sidebar.
4. Hover over the action button and click on the **gear** icon, and select **Change action**.
5. Click on the field’s dropdown to select where the action should get it’s value. Here you can select “Ask the user” or have it automatically populated by a dashboard filter. In this case, we’ll select our “ID” filter.

<figure><img src="https://www.metabase.com/docs/latest/dashboards/images/id-value.png" alt=""><figcaption></figcaption></figure>

If you also wire that filter up to a card with a model on the dashboard, you can filter for individual records in that model, view them in the model’s card, and have the action buttons auto-populate the id.

<figure><img src="https://www.metabase.com/docs/latest/dashboards/images/button-form.png" alt=""><figcaption></figcaption></figure>

You can add as many buttons as you want, and wire them up to one or more filters.
