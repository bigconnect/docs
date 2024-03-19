# Configuring User Import and Export

The import/export settings configure mappings between LDAP and Liferay to match users between the two systems.

### [Finding Users in Your LDAP Directory](https://learn.liferay.com/web/guest/w/dxp/users-and-permissions/connecting-to-a-user-directory/configuring-user-import-and-export#finding-users-in-your-ldap-directory) <a href="#finding-users-in-your-ldap-directory" id="finding-users-in-your-ldap-directory"></a>

**Authentication Search Filter:** Use this search filter box to determine the search criteria for user logins. By default, Liferay uses users’ email addresses for their login names. The value here must use the [authentication type](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/securing-liferay/authentication-basics#authentication-types) you use. For example, if you changed Liferay’s authentication method to use screen names instead of the email addresses, you would modify the search filter so it can match the entered log in name:

```log
(cn=@screen_name@)
```

**Import Search Filter:** Depending on the LDAP schema, there are different ways to identify the user. The default setting is usually fine:

```log
(objectClass=inetOrgPerson)
```

If you want to search for only a subset of users or users that have different LDAP object classes, you can change this.

### [Mapping LDAP User Attributes to Liferay Fields](https://learn.liferay.com/web/guest/w/dxp/users-and-permissions/connecting-to-a-user-directory/configuring-user-import-and-export#mapping-ldap-user-attributes-to-liferay-fields) <a href="#mapping-ldap-user-attributes-to-liferay-fields" id="mapping-ldap-user-attributes-to-liferay-fields"></a>

Next, you can define mappings from LDAP attributes to Liferay fields. Though LDAP user attributes may be different from LDAP server to LDAP server, there are five fields Liferay requires to be mapped for the user to be recognized:

* _Screen Name_ (e.g., `uid` or `cn`)
* _Password_ (e.g., `userPassword`)
* _Email Address_ (e.g., `mail` or `email`)
* _First Name_ (e.g., `name` or `givenName`)
* _Last Name_ (e.g., `sn`)

NOTE

If you intend to create or import users with no email addresses, you must set `users.email.address.required=false` in `portal-ext.properties`. With this set, Liferay auto-generates an email address combining the user ID plus the suffix defined in the property `users.email.address.auto.suffix=`. Finally, make sure to set Liferay and LDAP authentication to something other than email address.

If you want to import LDAP groups as Liferay user groups, make sure you define a mapping for the Liferay group field so that membership information is preserved:

* _Group_ (e.g., _member_)

The other LDAP user mapping fields are optional.

The Control Panel provides default mappings for commonly used LDAP attributes. You can also add your own mappings.

**Test LDAP Users:** Once you have your attribute mappings set up (see above), click the _Test LDAP Users_ button and Liferay attempts to pull LDAP users and match them with their mappings as a preview.

![You should see a list of users when you click the Test LDAP Users button.](https://resources.learn.liferay.com/images/dxp/latest/en/users-and-permissions/connecting-to-a-user-directory/configuring-user-import-and-export/images/01.png)

### [Mapping LDAP Groups to Liferay User Groups](https://learn.liferay.com/web/guest/w/dxp/users-and-permissions/connecting-to-a-user-directory/configuring-user-import-and-export#mapping-ldap-groups-to-liferay-user-groups) <a href="#mapping-ldap-groups-to-liferay-user-groups" id="mapping-ldap-groups-to-liferay-user-groups"></a>

This section contains settings for mapping LDAP groups to Liferay user groups.

**Import Search Filter:** This is the filter for mapping LDAP groups to Liferay user groups. For example,

```log
(objectClass=groupOfNames)
```

Enter the LDAP group attributes you want retrieved for this mapping. The following attributes can be mapped. The _Group Name_ and _User_ fields are required, the _Description_ is optional.

* _Group Name_ (e.g., `cn` or `o`)
* _Description_ (e.g., `description`)
* _User_ (e.g., `member`)

**Test LDAP Groups:** Click the _Test LDAP Groups_ button to display a list of the groups returned by your search filter.

### [Export](https://learn.liferay.com/web/guest/w/dxp/users-and-permissions/connecting-to-a-user-directory/configuring-user-import-and-export#export) <a href="#export" id="export"></a>

This section contains settings for exporting Liferay user data to LDAP.

**Users DN:** Enter the location in your LDAP tree where the users are stored. Liferay exports the users to this location.

**User Default Object Classes:** Users are exported with the listed default object classes. To find out what your default object classes are, use an LDAP browser tool such as Apache Directory Studio to locate a user and view the Object Class attributes stored in LDAP for that user.

**Groups DN:** Enter the location in your LDAP tree where the groups are stored. When Liferay does an export, it exports the groups to this location.

**Group Default Object Classes:** When a group is exported, the group is created with the listed default object classes. To find out what your default object classes are, use an LDAP browser tool such as [Apache Directory Studio](https://directory.apache.org/studio) to locate a group and view the Object Class attributes stored in LDAP for that group.

When you’ve set all your options and tested your connection, click _Save_.

NOTE

If a user changes a value like a password in Liferay, that change is passed to the LDAP server, provided Liferay has enough schema access to make the change.

Now you know how to connect an LDAP server to Liferay and how to configure user import behavior, export behavior, and other LDAP settings. There are other configurable options; [Configuring LDAP](https://learn.liferay.com/web/guest/w/dxp/users-and-permissions/connecting-to-a-user-directory/ldap-configuration-reference) describes those.
