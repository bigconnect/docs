# Managing Organizations

### Adding Organizations <a href="#adding-organizations" id="adding-organizations"></a>

Add an Organization (perhaps start by adding the _Physical Plant Department_ organization to the Lunar Resort):

1. Click _Users and Organizations_ from Control Panel → Users.
2. Go to the _Organizations_ tab and click the _Add_ button. Fill out the Name field at a minimum.
3. If you’re creating a child Organization, use the Parent Organization _Select_ button to select an Organization in the system to be the direct parent. Click the _Remove_ button to remove the currently configured parent.
4. Click _Save_ when finished filling out the Add Organization form.

As when creating a new user, once you submit the form a success message appears and you have access to a new form which lets you enter additional information about the Organization. Organizations can have associated multiple email addresses, postal addresses, web sites, and phone numbers. The Services link can be used to indicate the operating hours of the Organization, if any.

**Tip:** After creating an Organization, assign the desired user to the Organization Owner Role. The Organization Owner can do everything that an organization Administrator can. In addition to their full administrative rights within the Organization, they can do these things:

* Appoint other Users to be Organization Administrators
* Appoint other Users to be Organization Owners
* Remove the memberships of other Organization Administrators or Owners

Organization Administrators can’t make these Role assignments and can’t manage the memberships of other Organization Administrators or Owners.

### Editing Organizations <a href="#editing-organizations" id="editing-organizations"></a>

To edit an Organization, go to the Users and Organizations section of the Control Panel and click the _Organizations_ tab. All active Organizations are listed. Click the _Actions_ button next to an Organization. This shows a list of actions you can perform on this Organization.

* _Edit_ lets you specify details about the Organization, including addresses, phone numbers, and email addresses. You can also create a Site for the Organization.
* _Manage Site_ lets you create and manage the public and private pages of the Organization’s Site. This only appears after a Site has been created for the Organization.
* _Assign Organization Roles_ lets you assign Organization-scoped Roles to Users. By default, Organizations are created with three Roles: Organization Administrator, Organization User and Organization Owner. You can assign one or more of these Roles to Users in the Organization. All members of the Organization automatically get the Organization User Role so this Role is hidden when you click Assign Organization Roles.
* _Assign Users_ lets you search and select Users to be assigned to this Organization as members.
* _Add User_ adds a new User and assigns the User as a member of this Organization.
* _Add Organization_ lets you add a child Organization to this Organization. This is how you create hierarchies of Organizations with parent-child relationships.
* _Delete_ removes this Organization. Make sure the Organization has no Users in it first. You’ll be prompted for confirmation that you want to delete the Organization. If there are Users in the Organization or if there are sub-Organizations, you must remove the Users and delete the sub-Organizations before deleting the parent Organization.

If you click the Organization name you can view both a list of Users who are members of this Organization and a list of all the sub-Organizations of this Organization.

### Organization Types <a href="#organization-types" id="organization-types"></a>

By default, MWARE Portal only includes the _Organization_ type. Configure the existing type or add additional types using the aptly named Organization Type entry in System Settings. There are two main reasons to configure Organization types:

1. Organizations usually correlate to real-life hierarchical structures. Calling them by their real names is helpful for administrators and Users. In the Major League Baseball (MLB) example, _League_, _Division_, and _Team_ Organization types are useful.
2. Enforce control over which Organizations can be top level Organizations and the type of sub-Organization allowed for each parent Organization type. For example, MLB would not allow Division Organization types to be sub-Organizations of Team Organizations.

![Figure 1: Create new organization types through the System Settings entry called Organization Types.](https://liferay-support.zendesk.com/hc/article\_attachments/360029928592/orgs-organization-type.png)

Check out the configuration options that configure the default _Organization_ type and then configure an additional type.

To add another Organization type called _League_, enter these options into the configuration form:

Name: _League_Adds League to the list of Organization types that appear in the Add Organization menu.Country Enabled: _True_Enables the Country selection list field on the form for adding and editing League types.Country Required: _False_Specifies that the _Country_ field is not required when adding a League.Rootable: _True_Enables Leagues as a top level Organization. Limit League to sub-Organization status by excluding this property.Children Types: _Division_Specifies Division as the only allowable sub-Organization type for the League parent type.

Once you configure additional Organization types and click Save, you’ll find your new type(s) available for selection in the Add Organization form.

![Figure 2: Custom configuration types are available in the Add Organization form.](https://liferay-support.zendesk.com/hc/article\_attachments/360029928572/orgs-add-custom-organization.png)

Users can join or be assigned to Sites when they share a common interest. Users can be assigned to Organizations when they fit into a hierarchical structure. User groups provide a more ad hoc way to group users than sites and Organizations. You’ll look at them next.
