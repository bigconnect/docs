# Defining Object Actions

Object actions define operations that you can run manually or automatically for object entry events.

To add an action,

1. Open the _Global Menu_ ( ![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png) ), go to the _Control Panel_ tab, and click _Objects_.
2.  Begin editing an object definition.

    NOTE

    For Liferay 7.4 U22+/GA22+, you can add actions to system objects.
3. Go to the _Actions_ tab and click _Add_ ( ![Add Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png) ).
4.  In the Basic Info tab, enter a _Name_ and _Description_ for the action.

    ![Enter an action name and description in the Basic Info tab.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/actions/defining-object-actions/images/01.png)
5.  Go to the _Action Builder_ tab to define the custom trigger, conditions, and action.

    ![Go to the Action Builder tab to define a custom trigger, conditions, and action.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/actions/defining-object-actions/images/02.png)
6.  Specify a trigger event to determine when the action is performed.

    | Trigger                      | Description                            |
    | ---------------------------- | -------------------------------------- |
    | On After Add                 | When an entry is added                 |
    | On After Attachment Download | When an entry attachment is downloaded |
    | On After Delete              | When an entry is deleted               |
    | On After Update              | When an entry is updated               |
    | Standalone                   | Triggered manually                     |

    All triggers besides standalone are automatic. Standalone triggers are for running any type of action manually. See [Using Manual Actions](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/actions/using-manual-actions) for more information.

    ![Determine when the action is performed.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/actions/defining-object-actions/images/03.png)
7.  (Optional) Enable a condition for automatic triggers. Click _Code_ ( ![Code](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-code.png) ) to view available expressions.

    On Liferay 7.4 U33+/GA33+, Liferay validates the condition’s syntax when you click _Done_. If the expression is invalid, Liferay displays an error message. See [Expression Builder Validations Reference](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/validations/expression-builder-validations-reference) for more information on available fields, operators, and functions.

    ![Use expressions to set additional conditions for the trigger.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/actions/defining-object-actions/images/04.png)
8.  Select the type of action to perform and fill out all additional fields.

    | Type                   | Description                                                   |
    | ---------------------- | ------------------------------------------------------------- |
    | Notification           | Send email or user notifications using a predefined template. |
    | Add an Object Entry    | Create entries in an active object.                           |
    | Update an Object Entry | Update fields in the current object entry.                    |
    | Webhook                | Deliver a payload to a URL.                                   |
    | Groovy Script          | Execute custom [Groovy](https://groovy-lang.org/) scripts.    |

    IMPORTANT

    Groovy script actions are only available for Liferay PaaS and Liferay DXP Self-Hosted.

    See [Understanding Action Types](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/actions/understanding-action-types#understanding-action-types) for more information.
9. Click _Save_.

Once created, the action is active and triggered according to your configuration.

You can deactivate an action from its Basic Info tab. Toggle the _Active_ switch and click _Save_.

![Toggle the Active switch to deactivate the action.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/actions/defining-object-actions/images/05.png)
