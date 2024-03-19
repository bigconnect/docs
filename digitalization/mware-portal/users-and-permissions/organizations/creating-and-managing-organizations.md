# Creating and Managing Organizations

### [Adding Organizations](https://learn.liferay.com/web/guest/w/dxp/users-and-permissions/organizations/creating-and-managing-organizations#adding-organizations) <a href="#adding-organizations" id="adding-organizations"></a>

1. Click _Users and Organizations_ from Control Panel → Users.
2.  Go to the _Organizations_ tab and click the Add button (![Add](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png)). Fill out the Name field at a minimum.

    ![Adding a new organization requires the Name field.](https://resources.learn.liferay.com/images/dxp/latest/en/users-and-permissions/organizations/creating-and-managing-organizations/images/01.png)
3. If you’re creating a child Organization, use the Parent Organization _Select_ button to select an Organization in the system to be the direct parent. Click the _Remove_ button to remove the currently configured parent.
4. Click _Save_ when finished filling out the Add Organization form.

Once you submit the form, you see a success message, and a new form appears where you can enter additional information about the Organization. Organizations can have associated multiple email addresses, postal addresses, web sites, and phone numbers. You can use the Services link to indicate the operating hours of the Organization, if any.

TIP

After creating an Organization, assign a user to the Organization Owner Role. The Organization Owner can do everything that an Organization Administrator can, within the context of the assigned organization. In addition, Organization Owners can do these things:

* Appoint other Users to be Organization Administrators
* Appoint other Users to be Organization Owners
* Remove the memberships of other Organization Administrators or Owners

Organization Administrators can’t make these Role assignments and can’t manage the memberships of other Organization Administrators or Owners.

### [Editing Organizations](https://learn.liferay.com/web/guest/w/dxp/users-and-permissions/organizations/creating-and-managing-organizations#editing-organizations) <a href="#editing-organizations" id="editing-organizations"></a>

1. Go to the Users and Organizations section of the Control Panel
2. Click the _Organizations_ tab. All active Organizations are listed.
3. Click the _Actions_ button next to an Organization. This shows a list of actions you can perform on this Organization:

**Edit:** Specify details about the Organization, including addresses, phone numbers, and email addresses. You can also create a Site for the Organization.

**Manage Site:** Create and manage Pages in the Organization’s Site. This only appears after a Site has been created for the Organization.

**Assign Organization Roles:** Assign Organization-scoped Roles to Users. By default, Organizations are created with three Roles: Organization Administrator, Organization User and Organization Owner. You can assign one or more of these Roles to Users in the Organization. All members of the Organization automatically get the Organization User Role so this Role is hidden when you click Assign Organization Roles.

**Assign Users:** Search and select Users to be assigned to this Organization as members.

**Add Organization:** Add a child Organization to this Organization. This is how you create hierarchies of Organizations with parent-child relationships.

**Delete:** Removes this Organization. Make sure the Organization has no Users in it first. You’ll be prompted for confirmation that you want to delete the Organization. If there are Users in the Organization or if there are sub-Organizations, you must remove the Users and delete the sub-Organizations before deleting the parent Organization.

If you click the Organization name you can view both a list of Users who are members of this Organization and a list of all the sub-Organizations of this Organization.

### [Viewing Organizations](https://learn.liferay.com/web/guest/w/dxp/users-and-permissions/organizations/creating-and-managing-organizations#viewing-organizations) <a href="#viewing-organizations" id="viewing-organizations"></a>

You can view the hierarchical structure of organizations by going into the _Organization Chart_ tab. This displays all the currently available organizations. See [Using the new Organization Management Chart Widget](https://learn.liferay.com/w/commerce/creating-store-content/liferay-commerce-widgets/using-the-new-organization-management-chart-widget) for more information.
