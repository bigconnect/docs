# Users

Give access to individual users then manage them as their needs change over time.

## Add a new user

From the menu select **Users**.  Enter a username and a strong password (and confirm it). You can also set whether this user is an Administrator as well as add the user to any teams you have created.&#x20;

![](<../../../.gitbook/assets/image (772).png>)

Once you're ready, click **Create user**.

## Turn a user into an administrator

From the menu select **Users** then select the user you want to promote to administrator. Toggle **Administrator** on then click **Save**.

![](<../../../.gitbook/assets/image (726).png>)

## Reset a user's password

From the menu select **Users** then select the user whose password you want to reset. Enter a new strong password, re-enter the password to confirm it then click **Update password**.

![](<../../../.gitbook/assets/image (736).png>)

## Authentication

BigConnect VM provides its own internal authentication mechanism, encrypting user passwords and storing them in the local BigConnect VM database. Alternatively, external authentication providers are available. In this section, we explain how to authenticate via LDAP, Active Directory and OAuth.

When using internal authentication, an administrator can set the minimum length for users' passwords. The default is 12 characters, but this can be adjusted using the slider. Any users with passwords that don't meet the requirements will be asked to update their passwords when they next log in.

![](<../../../.gitbook/assets/image (750).png>)

### Authenticate via LDAP

BigConnect VM can be configured to accept Lightweight Directory Access Protocol (LDAP) authentication if your organization has implemented LDAP authentication. When users attempt to log into BigConnect VM, the application will authenticate them against your LDAP directory. If authentication is successful, the user is allowed to log into BigConnect VM.

To configure BigConnect VMLDAP authentication, you first need to add a user to your directory service for the purpose of authenticating from BigConnect VMto read the LDAP. The user should be a service account that needs read-only access to LDAP.

#### Enabling LDAP

Log into Portainer as an administrator. From the menu select **Settings**, select **Authentication** then select the **LDAP Authentication** option. Extra fields will appear, allowing you to configure LDAP.

#### Automatic user provisioning

Enabling this setting automatically creates users within BigConnect VM once they are successfully authenticated by LDAP. If you do not enable this, you must manually create users with the same username as the corresponding LDAP directory.

![](<../../../.gitbook/assets/image (754).png>)

#### Server Type

Here you can select a custom configuration or a preconfigured OpenLDAP template.

![](<../../../.gitbook/assets/image (729).png>)

#### LDAP configuration

Enter the IP address/FQDN and port number of your LDAP server. Opt to either connect anonymously (your LDAP server must support this) or enter a user account that has READ access to the directory. Click **Test connectivity** to validate that you can connect.

![](<../../../.gitbook/assets/image (761).png>)

If you want to add additional LDAP servers to provide for authentication fallback, click **Add additional server** and fill in the server details.

#### LDAP security

Configure the remaining LDAP settings, using the table below as a guide:

![](<../../../.gitbook/assets/image (751).png>)

#### User-search configurations

**BaseDN**

* Enter `dc=mydomain,dc=com` to search your entire directory for the username attempting to login.
* Enter `ou=myou,dc=mydomain,dc=com` to search for users only within the specified OU.
* Enter `cn=mycn,dc=mydomain,dc=com` if your users are only in a container.

If you have a large number of users in your domain, narrow the scope Portainer searches on by using OUs.

**Username attribute**

For LDAP, enter `uid` unless your configuration differs.

#### Filter

{% hint style="info" %}
These entries are case sensitive.
{% endhint %}

Enter filter criteria for the results returned from LDAP to BigConnect VM. For example, to only allow users who are members of a group defined within an OU to login, set **Filter** to the following (the brackets are important, so copy the entire string):

```
(&(objectClass=user)(memberOf=cn=mycn,ou=myou,dc=mydomain,dc=com))
```

In the example below, the domain `portainer.local` has an OU called `Groups` and within that OU is a group called `PortainerDevUsers`. This search filter will only allow users who are members of the `PortainerDevUsers` LDAP group to log into BigConnect VM.

![](<../../../.gitbook/assets/image (760).png>)

As an optional step, click **Add user search configuration** to define additional user-search configurations.

#### Group-search configurations

In addition to user search, BigConnect VM also gives you the option to set up group search. When configured, if an LDAP user is a member of an LDAP group, and that LDAP Group corresponds to an identically named BigConnect VM Team, then the LDAP user will automatically be placed into the Portainer Team based on their LDAP group membership. This is very useful for automatically granting access to BigConnect VMenvironments via group membership.

**Group Base DN**

Enter either:

* Enter `dc=mydomain,dc=com` to search your entire directory for the list of groups.
* Enter `ou=myou,dc=mydomain,dc=com` to search for groups only within the specified OU.
* Enter `cn=mycn,dc=mydomain,dc=com` if your groups are only in a container.

If you have a large number of groups in your domain, narrow the scope Portainer searches on by using OUs.

![](<../../../.gitbook/assets/image (776).png>)

**Group Membership Attribute**

Enter `member` as the attribute that determines if a user is a member of a group.

**Group Filter**

If you want to filter the list of groups to return only those that contain the string `Portainer` (for example: `PortainerDev`, `PortainerProd`, `PortainerUAT`), set up the filter like this:

```
(&(objectclass=group)(cn=*Portainer*))
```

![](<../../../.gitbook/assets/image (711).png>)

As an optional step, click **Add group search configuration** to define additional group-search configurations.
