# Using the Calendar Widget

The calendar widget displays a small monthly calendar showing an overview of upcoming events. A larger area shows the Scheduler, a more detailed calendar with a number of options: you can set it to to display a _Day_, _Week_, or _Month_, or choose a more event-oriented _Agenda_ setting.

![Figure 1: The default view is set in configuration, but a user can change it at any time.](https://liferay-support.zendesk.com/hc/article\_attachments/360030164731/calendar-view.png)

Two calendars are included by default when the widget is first added to a page: a personal calendar for the current user and a Site calendar for the current Site. These are displayed in the widget’s lower left. Next to each calendar is a colored box: click it to show/hide that calendar’s events in the main viewing area.

### Adding New Calendars <a href="#adding-new-calendars" id="adding-new-calendars"></a>

To create a new personal calendar,

1. Click on the arrow to the right of the _My Calendars_ header and select _Add Calendar_ from the menu.
2. Fill in the _Add Calendar_ form. Give the calendar a name and description, set a time zone, and decide if it’s your user’s _default calendar_—the one that is shown automatically whenever the widget is displayed. You can also pick a color, which color codes events whenever multiple calendar’s events are displayed at once. You can also decide to enable ratings or comments on the calendar’s events, and configure permissions.

To edit an existing calendar instead of adding a new one, select _Manage Calendars_ from the menu.

To add or edit a Site calendar, open the menu next to the header with the Site’s name.

![Figure 2: Personal and Site calendars are shown in the lower left. This image shows calendars belonging to User Test Test and Site Liferay DXP.](https://liferay-support.zendesk.com/hc/article\_attachments/360030196772/new-calendar-manage-calendars.png)

### Adding Events to a Calendar <a href="#adding-events-to-a-calendar" id="adding-events-to-a-calendar"></a>

To add events to a calendar,

1.  Click on any day in the main viewing area to open an event creation pop up. If you’ve selected the _Day_ or _Week_ view, you can click on the specific time when your event begins.

    <img src="https://liferay-support.zendesk.com/hc/article_attachments/360030164711/new-calendar-event-popup.png" alt="Figure 3: When you click anywhere on the calendar, youll see the event creation pop up appear. Click Edit to specify details for your event." data-size="original">

    Figure 3: When you click anywhere on the calendar, you'll see the event creation pop up appear. Click \*Edit\* to specify details for your event.
2.  Name your event and assign it to a calendar. Click _Save_ to create the event immediately or _Edit_ to enter additional information.

    <img src="https://liferay-support.zendesk.com/hc/article_attachments/360030164771/new-calendar-event-details.png" alt="Figure 4: You can specify event details such as the event title, start date, end date, description, location, and more." data-size="original">

    Figure 4: You can specify event details such as the event title, start date, end date, description, location, and more.
3.  If you clicked _Edit_, complete the _Edit Event_ form. Enter start and end times and enter a description. To schedule an event that reoccurs, check the _Repeat_ box and fill in the _Repeat_ pop up.

    <img src="https://liferay-support.zendesk.com/hc/article_attachments/360030164751/new-calendar-event-repeat.png" alt="Figure 5: The Repeat box allows you to specify whether an events repeats daily, weekly, monthly, or yearly, how often it repeats, and when (or if) it ends." data-size="original">

    Figure 5: The \*Repeat\* box allows you to specify whether an events repeats daily, weekly, monthly, or yearly, how often it repeats, and when (or if) it ends.

#### ADDITIONAL EVENT FUNCTIONS <a href="#additional-event-functions" id="additional-event-functions"></a>

At the bottom of the _Edit Event_ form, there are several collapsed sections: _Details_, _Invitations_, _Reminders_, _Categorization_, and _Related Assets_.

**Details**

In the Details section, you can move the event to another calendar and enter a location.

**Invitations**

In the invitations section, invite Users, Sites, or Calendar Resources (see the next tutorial for more on resources: in brief, a resource is anything you might need for an event—a conference room, a vehicle, etc.). Follow these steps:

1. Enter the name of an invitee (User, Site, or Resource) in the _Invitations_ field. Hit _Enter_ to add them to the _Pending_ column.
2. Check the availability of invitees by clicking the arrow next to the their names and selecting _Check Availability_. This displays their calendars (assuming you have permission to view them).

An automated email is sent to invitees who must navigate to the calendar widget to respond. See below to customize the content of the invitation.

When invitees respond to the invitation, their names move to the _Accepted_, _Declined_, or _Maybe_ columns.

**Reminders**

Schedule up to two email reminders to send to attendees. Reminders translate the time of the event into the recipients own time zone.

See below to customize the content of the reminder email.

**Categorization**

Tag your event or assign it to a category so it appears in appropriate search results and is published by any asset publisher set to publish content assigned to the same category.

**Related Assets**

List an asset—such as an agenda or supplementary material for a meeting—as related to your event. Links to related assets are displayed in the _Event Details_ window.

**Saving and Drafting Changes and Updating Permissions**

At the very bottom of the Edit form is a set of buttons that let you publish the changes, save the changes as a draft, and configure the event’s permissions.

Giving a user permission to add, delete, or update discussion allows them to make, edit and remove comments on the event. The _Permissions_ permission allows a Role to update permissions for the event.

### Customizing Email Notifications <a href="#customizing-email-notifications" id="customizing-email-notifications"></a>

To customize email notifications for event invitations and reminders,

1.  From the Calendar widget, click on the arrow next to a calendar and select _Calendar Settings_.

    <img src="https://liferay-support.zendesk.com/hc/article_attachments/360030196792/calendar-email-note.png" alt="Figure 6: Email templates apply to a single calendar and all its events." data-size="original">

    Figure 6: Email templates apply to a single calendar and all its events.
2. Click on the _Notification Templates_ tab. Then select either the _Invite Email_ or the _Reminder Email_ sub-tab.
3. Edit the email as desired. At the bottom of the screen is a glossary that specifies variables for terms that were set when you created the event. Use these variables to refer to event-specific information, such as the event’s name, date or location. It’s a good idea to include a link to the event (use the variable `[$EVENT_URL$]`) as users must navigate to the calendar widget in order to respond.

Click _Save_. Now your notifications contain the proper text.

The next article covers setting up calendar resources and porting data from one installation to another.
