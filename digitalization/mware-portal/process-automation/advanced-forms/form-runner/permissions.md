# Permissions

You can control access to specific form fields based on the user user's roles.

### Using and accessing roles <a href="#using-and-accessing-roles" id="using-and-accessing-roles"></a>

The `$fr-roles` XPath variable can be used in formulas controlling whether a field or section is visible or readonly. `$fr-roles` contains the list (as an XPath sequence) of roles of the current user, if any. Each role is represented as a string.

You can make a control non-visible to the current user by defining a _visibility_ expression that returns `false()`. If the control is visible, you can make it readonly to current user by defining a _readonly_ expression that returns `true()`.

### Examples <a href="#examples" id="examples"></a>

The following "Visibility" expression makes a section visible only if one of the roles has value `admin`:

```
fr:user-roles() = 'admin'
```

Due to the logic of XPath comparison on sequences, this expression returns `true()` if at least one of the roles is `admin`, even if there are other roles available.

You can restrict which users can access which forms, and what operations they can perform. Those restrictions apply to the forms you create once they are deployed, not to editing those forms in Form Builder

### Enabling permissions <a href="#enabling-permissions" id="enabling-permissions"></a>

By default, no restriction is imposed on _who_ can do _what_ with forms you create in Form Builder. You enable permissions by going to the Form Builder toolbox, and under _Advanced_, clicking on _Permissions_.

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-6792fc8450886ca68dd55af58966e8f411c03d84%252Fadvanced-menu.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=c039813767652bbadfcfb4099a826517472a86b8f50e0350b4ca02727157c4d0)

Opening the Permissions dialog

This shows the following dialog:

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-baca75df1e3a635cd03048e5df37c949a5ae1ddb%252Fpermissions-enable.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=c6d58cc8907f627b1270400d9c2030064f276ba85c9577b64a0c07167433df96)

Permissions dialog with no permissions enabled

After you click on the checkbox, you'll be able to set access restrictions on the _Create_, _Read_, _Update_, and _Delete_ operations.

### Setting permissions <a href="#setting-permissions" id="setting-permissions"></a>

#### An example <a href="#an-example" id="an-example"></a>

In the example below:

* Any user (even anonymous) can fill out new form data.
* A logged-in user who created form data can later read and update it.
* A logged-in user member of the group of the user who created form data can read it but not update it.
* Users with the role _clerk_ can read any form data. They can also list data on the Summary page.
* Users with the role _admin_ can do any operation, including deleting form data and listing data on the Summary page.

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-ea0ebfb4bb3f05c1224e2add61982f4c733dd09a%252Fpermissions-dialog.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=f150fa29147fe13da5804fdca2ea29379735e9408873490aa4b0f4f2b523e32f)

Permissions dialog with explicit permissions enabled

#### Role types <a href="#role-types" id="role-types"></a>

1. On the _Anyone_ line, set the operations allowed to all users.
2. On the _Require Token_ line, specify that the _Read_ and _Update_ permissions above require a token to be satisfied.
3. On the _Any Authenticated User_ line, set the operation allowed to authenticated users only.
4. On the _Owner_ line, set the operations allowed to the user who created the data.
5. On the _Group members_ line, set the operations allowed to users in the same group as the owner.&#x20;
6. On the following lines, you can enter a role name, and define what operations users with that role can perform.

#### Permissions in detail <a href="#permissions-in-detail" id="permissions-in-detail"></a>

Permissions you set in the dialog are _additive_ – Say you defined permissions for two roles, where users with the _reader_ role can read and users in the _clerk_ role can delete, users with both roles ( _reader_ and _clerk_) are allowed to perform both operations (reading and deleting).

Operation on _Anyone_ apply to all other rows – When you select a checkbox for a given operation on the first _Anyone_ row, that checkbox will be automatically checked and disabled so you can't change it, for any additional row, since you wouldn't want to authorize users with additional roles to perform less operations.

_Update_ implies _Read_ – On any row, if you check _Update_, then _Read_ will be automatically checked, as it wouldn't make sense to say that some users can update data, but can't read it, as when updating data, obviously, they must be shown the data they are updating.

_Create_ can't be set for the _Owner_ and _Group members_ – The owner/group is a piece of information attached to existing form data, keeping track of the user who create the data, and the group in which this user is. This information is only known for existing data, so assigning the _Create_ permission to the _Owner_ or _Group members_ doesn't make sense, and the dialog doesn't show that checkbox.

Permissions for the _Owner_ and _Group members_ can be set independently – If you want data to be accessible only by people who created it, check Read/Update/Delete/List for the owner but not for group members. If you want data to be accessible by all people in the same group, check Read/Update/Delete/List for the group members and don't check them for the owner if you want the owner to lose access to that data in case the owner changes group. (The latter highlights the need for permissions owner and group member to be set independently.)

#### The _List_ permission <a href="#the-list-permission" id="the-list-permission"></a>

The _List_ permission allows specifying that the user can list form data on the Form Runner Summary page. If a user navigates to the Summary page of a form and the _List_ permission is not granted to the user, the Summary page responds with an "Unauthorized" error.

With earlier versions of Advanced Forms, the _List_ permission was not checked separately. However, the ability to "Read" implied the ability to _List_ on the Summary page.

Forms created and edited with earlier versions of Advanced Forms that have the _Read_ permission enabled also implicitly have the _List_ permission enabled, for backward compatibility. Opening such a form in Form Builder will show both the _Read_ and _List_ permissions. If the form author deselects the _List_ permission, and then publishes the form, then the form will not allow the _List_ permission, as expected.

### Per-app and global permissions <a href="#per-app-and-global-permissions" id="per-app-and-global-permissions"></a>

#### Introduction <a href="#introduction-1" id="introduction-1"></a>

You can configure permissions per app, as well as globally. This is particularly useful when you have a large number of forms, and want to configure permissions for all of them at once. Per-app and global permissions are currently set using configuration properties in `properties-local.xml`.

#### Properties <a href="#properties" id="properties"></a>

You configure permissions with the `oxf.fr.permissions.$app.$form` properties. For example:

Copy

```
<property as="xs:string"  name="oxf.fr.permissions.acme.sales">
  {
    ...
  }
</property>
```

As usual, you can use wildcards to specify whether the configuration is global:

Copy

```
<property as="xs:string"  name="oxf.fr.permissions.*.*">
  {
    ...
  }
</property>
```

Or whether it applies to a specific app:

Copy

```
<property as="xs:string"  name="oxf.fr.permissions.acme.*">
  {
    ...
  }
</property>
```

#### Precedence of permissions configurations <a href="#precedence-of-permissions-configurations" id="precedence-of-permissions-configurations"></a>

If a form definition includes a Form Builder-defined set of permissions, then those permissions are used and the permissions set in `properties-local.xml` are ignored.

There is no merging of permissions between a Form Builder-defined set of permissions and properties-defined permissions, or between global and per-app permissions.

#### Configuration detail <a href="#configuration-detail" id="configuration-detail"></a>

For each property, the configuration is a JSON format that follows the layout of the Form Builder user interface (see screenshot above):

Copy

```
{
  "anyone":         [ "create" ],
  "owner":          [ "read", "update" ],
  "group-member":   [ "read", "update" ],
  "roles": {
    "forms-user":  [ "read", "update", "list" ],
    "forms-admin": [ "read", "update", "delete", "list" ]
  }
}
```

The configuration is a JSON object with the following properties:

* `anyone`: an array of operations that are allowed for all users
* `anyone-with-token`:
  * an array of operations that are allowed for users with a link including a token
* `any-authenticated-user`
  * an array of operations that are allowed for users who are authenticated
  * this excludes, therefore, anonymous users
* `owner`: an array of operations that are allowed for the user who created the data
* `group-member`: an array of operations that are allowed for users in the same group as the owner of the data
* `roles`: a JSON object with the following properties:
  * each key is a role name
  * each value is an array of operations that are allowed for users with the given role

The operations are the same as the ones in the Form Builder user interface:

* `create`: create new data
* `read`: read data
* `update`: update data
* `delete`: delete data
* `list`: list data on the Summary page

To indicate that no operation is allowed:

* use an empty array
* or omit the property

#### Impact on versioning <a href="#impact-on-versioning" id="impact-on-versioning"></a>

For permissions defined in Form Builder, the permissions are stored in the form definition, and this means that you can have multiple form versions published with different permissions.

However, for permissions defined in `properties-local.xml`, the permissions are per app/form name, and will apply to all published versions of the form under that app/form name.

### How permissions affect Form Runner pages <a href="#how-permissions-affect-form-runner-pages" id="how-permissions-affect-form-runner-pages"></a>

#### Introduction <a href="#introduction-2" id="introduction-2"></a>

Which operations the current user can perform drives what page they can access, and on some pages which buttons are shown.

#### Published Forms page <a href="#published-forms-page" id="published-forms-page"></a>

This was formerly known as the _Home_ page.

On the Form Runner Published Forms page, all the forms on which the current user can perform at least one operation are displayed. Then, for each one of those forms:

* If they can perform the _Create_ operation on the form, a link to the _New_ page is shown.
* If they can perform any of the _Read_, _Update_, or _Delete_ operation on the form, a link to the _Summary_ page for that form is shown.

#### Summary page <a href="#summary-page" id="summary-page"></a>

* Access is completely denied if the current user can't perform any of the _Read_, _Update_, or _Delete_ operations. Access is also completely denied if the user doesn't have the _List_ permission.
* The _Delete_ button is disabled if the current user can't perform the _Delete_ operation.
* The _review_ and _pdf_ button are disabled if the current user can't perform the _Read_ operation.
* Clicking in a row of the table will open the form in _Edit_ mode if the current user can perform the _Update_ operation, in _View_ mode if they can perform the _Read_ operation, and do nothing otherwise.

#### View, New, and Edit pages <a href="#view-new-and-edit-pages" id="view-new-and-edit-pages"></a>

* For the _View_ page, access is denied if the current user can't perform the _Read_ operation.
* For the _New_ page, access is denied if the current user can't perform the _Create_ operation.
* For the _Edit_ page, access is denied if the current user can't perform the _Update_ operation.
