# Configuring LDAP Authentication

If your organization uses LDAP and you want multiple users to access BigConnect Pipelines you can configure BigConnect Pipelines to use LDAP authentication. After you configure LDAP authentication, users log in to BigConnect Pipelines using their LDAP username and password.

To configure LDAP authentication, perform the following tasks:

1. Configure LDAP connection information.
2. Optionally, configure secure connections to the LDAP server.
3. Map LDAP groups to BigConnect Pipelines roles.
4. Optionally, configure multiple LDAP servers.
5. If you use MapR stages, enable LDAP authentication for MapR.

#### Step 1. Configure LDAP Connection Information <a href="#ariaid-title3" id="ariaid-title3"></a>

To enable LDAP authentication, configure LDAP connection information in the BigConnect Pipelines configuration files, `sdc.properties` and `ldap-login.conf`, located in the `$SDC_CONF` directory.

1. In the BigConnect Pipelines configuration file, `$SDC_CONF/sdc.properties`, enable LDAP authentication by setting the **http.authentication.login.module** property to **ldap**.
2.  In the `$SDC_CONF/sdc.properties` file, define the HTTP authentication type by setting the **http.authentication** property to **basic**, **digest**, or **form**.\


    The HTTP authentication type determines how passwords are transferred from the browser to BigConnect Pipelines over HTTP. Digest authentication encrypts user credentials. Basic and form authentication transfer user credentials as is. To ensure that user credentials are safely transmitted for basic and form authentication, configure BigConnect Pipelines to use HTTPS and configure secure connections to LDAP using LDAPS or StartTLS.\


    For a Microsoft Active Directory server, use basic or form authentication.\

3.  In the `$SDC_CONF/ldap-login.conf` file, configure the connection information for the LDAP server.In the file, configure the following properties:

    | LDAP Property         | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
    | --------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | debug                 | Enables LDAP debugging. Default is false.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
    | useLdaps              | <p>Secure LDAP connections using the LDAPS (LDAP over SSL) protocol. Default is false.<br></p><p>You must complete additional steps to use LDAPS, see Step 2. Configure Secure Connections to LDAP (Optional).</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
    | useStartTLS           | <p>Secure LDAP connections using the StartTLS protocol. Default is false.<br><br>You must complete additional steps to use StartTLS, see Step 2. Configure Secure Connections to LDAP (Optional).<br><br><strong>Note</strong>: StartTLS and LDAPS cannot be used at the same time. If both useStartTLS and useLdaps are set to true, useStartTLS takes precedence.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
    | contextFactory        | LDAP initial context factory. Default is com.sun.jndi.ldap.LdapCtxFactory.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
    | hostname              | LDAP server host name.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
    | port                  | <p>LDAP server port.<br></p><p>To use unencrypted connections or to use connections secured with StartTLS, enter the LDAP port number, typically 389. To use connections secured with LDAPS, enter the port number for secure connections, typically 636.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
    | bindDn                | Root distinguished name (DN) used to query the directory server. This user must have privileges to search the directory.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
    | bindPassword          | <p>Password for the root distinguished name. For additional security, save the password in the <code>$SDC_CONF/ldap-bind-password.txt</code> file without additional characters, spaces, or line breaks. As a best practice, the file should have owner-only permissions.</p><p>Default is @ldap-bind-password.txt@, which points to the <code>ldap-bind-password.txt</code> file for the password.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                |
    | forceBindingLogin     | <p>Determines if binding login checks are performed. Use one of the following options:</p><ul><li><p>false - BigConnect Pipelines performs the authentication based on information received from the LDAP server.</p><p><br>When set to false, the bindDn user must have permission to access the details, password, and group information for users. Use for digest authentication.</p></li><li><p>true - BigConnect Pipelines passes the user credentials to the LDAP server for authentication.</p><p><br>When set to true, the bindDn user must have permission to access the group information for users. Use for a Microsoft Active Directory server, for basic or form authentication, or when the password stored in the LDAP server is encrypted. Do not use for digest authentication.</p></li></ul><p>Default is false.</p> |
    | userBaseDn            | Base DN under which user accounts are located.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
    | userIdAttribute       | Name of the user ID attribute. Default is uid.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
    | userPasswordAttribute | <p>Name of the attribute where the user password is stored. Default is userPassword.</p><p>For a Microsoft Active Directory server, use an empty string.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
    | userObjectClass       | Name of the user object class. Default is inetOrgPerson.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
    | userFilter            | Name of the user attribute used to log in to BigConnect Pipelines. For example, LDAP users can log in using a username, uid, or email address. Default is `uid={user}`.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
    | roleBaseDn            | Base DN to search for role membership.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
    | roleNameAttribute     | Name of the attribute for role names. Default is roleName.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
    | roleMemberAttribute   | Name of the role attribute for user names. Default is Member.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
    | roleObjectClass       | Role object class. Default is groupOfNames.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
    | roleFilter            | <p>Specifies whether LDAP group members are determined by DN or uid. Enter one of the following values:</p><ul><li><code>"member={dn}"</code> - Use when LDAP groups list members by full DN.</li><li><code>"memberUid={user}"</code> - Use when LDAP groups list members by uid.</li></ul><p>Default is DN.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |

#### Example for OpenLDAP <a href="#ariaid-title4" id="ariaid-title4"></a>

Let's look at an example `ldap-login.conf` file and see how BigConnect Pipelines uses the LDAP connection information to authenticate LDAP users.

The following example shows a `ldap-login.conf` file for an OpenLDAP server:

```
ldap {
  com.streamsets.datacollector.http.LdapLoginModule required
  debug="true"
  useLdaps="true"
  useStartTLS="false"
  contextFactory="com.sun.jndi.ldap.LdapCtxFactory"
  hostname="server1"
  port="636"
  bindDn="cn=admin,dc=example,dc=net"
  bindPassword="@ldap-bind-password.txt@"
  forceBindingLogin="false"
  userBaseDn="ou=users,dc=example,dc=net"
  userIdAttribute="uid"
  userPasswordAttribute="userPassword"
  userObjectClass="inetOrgPerson"
  userFilter="uid={user}"
  roleBaseDn="ou=groups,dc=example,dc=net"
  roleNameAttribute="cn"
  roleMemberAttribute="member"
  roleObjectClass="groupOfNames"
  roleFilter="member={dn}";
}; 
```

When an LDAP user logs into BigConnect Pipelines, BigConnect Pipelines uses the connection information in the `ldap-login.conf` file to authenticate the user. BigConnect Pipelines completes the following steps to authenticate the LDAP user:

1.  When forceBindingLogin is set to false, checks if the user account is registered in the configured LDAP server by sending the following query to the LDAP server:

    ```
    ldapsearch -LLL -H ldaps://<hostname>:<port> -x -D <bindDn> -w <bindPassword> -b <userBaseDn> "(&(objectClass=<userObjectClass>)(<userIdAttribute>=username))"
    ```

    \
    For example, let's use the sample `ldap-login.conf` file configured above, assume that the password defined in `ldap-bind-password.txt` is "password", and assume that a user logs in to BigConnect Pipelines with the username of `jdoe`. BigConnect Pipelines sends the following query to the LDAP server:

    ```
    ldapsearch -LLL -H ldaps://server1:636 -x -D "cn=admin,dc=example,dc=net" -w password -b "ou=users,dc=example,dc=net" "(&(objectClass=inetOrgPerson)(uid=jdoe))"
    ```

    \
    If the user account doesn't exist, BigConnect Pipelines fails the authentication. If the user account exists, BigConnect Pipelines continues with the next authentication step.\


    **Note**: When forceBindingLogin is set to true, BigConnect Pipelines does not send this query to the LDAP server. Instead, BigConnect Pipelines passes the user credentials to the LDAP server for authentication. If the LDAP server successfully authenticates the user account, BigConnect Pipelines continues with the next authentication step.
2.  Checks which LDAP group the user account belongs to by sending the following query to the LDAP server:

    ```
    ldapsearch -LLL -H ldaps://<hostname>:<port> -x -D <bindDn> -w <bindPassword> -b <roleBaseDn> "(&(objectClass=<roleObjectClass>)(member={dn}))"
    ```

    \
    For example, using the sample `ldap-login.conf` file configured above, BigConnect Pipelines sends the following query to the LDAP server:

    ```
    ldapsearch -LLL -H ldaps://server1:636 -x -D "cn=admin,dc=example,dc=net" -w password -b "ou=groups,dc=example,dc=net" "(&(objectClass=groupOfNames)(member="cn=jdoe,ou=users,dc=example,dc=net"))"
    ```

    \
    The LDAP server returns the names of the LDAP groups that the user belongs to. BigConnect Pipelines uses the group names to determine the BigConnect Pipelines roles mapped to the LDAP groups, as explained in Step 3. Map LDAP Groups to BigConnect Pipelines Roles.

#### Example for Active Directory <a href="#ariaid-title5" id="ariaid-title5"></a>

The following example shows a `ldap-login.conf` file for a Microsoft Active Directory server. BigConnect Pipelines completes the same steps to authenticate LDAP users in Active Directory as in OpenLDAP.

```
ldap {
     com.streamsets.datacollector.http.LdapLoginModule required
     debug="true"
     useLdaps="true"
     useStartTLS="false"
     contextFactory="com.sun.jndi.ldap.LdapCtxFactory"
     hostname="*******"
     port="636"
     bindDn="********"
     bindPassword="@ldap-bind-password.txt@"
     forceBindingLogin="true"
     userBaseDn="ou=Department,dc=Company,dc=net"
     userIdAttribute="sAMAccountName"
     userPasswordAttribute=""
     userObjectClass="person"
     userFilter="sAMAccountName={user}"
     roleBaseDn="ou=Department,dc=Company,dc=net"
     roleNameAttribute="cn"
     roleMemberAttribute="member"
     roleObjectClass="group"
     roleFilter="member={dn}";
};
```

#### Step 2. Configure Secure Connections to LDAP (Optional) <a href="#ariaid-title6" id="ariaid-title6"></a>

You can optionally configure BigConnect Pipelines to use one of the following methods to make secure connections to the LDAP server: \
\
**LDAP over SSL (LDAPS)**\
\
LDAPS uses SSL to encrypt LDAP connections. LDAPS uses the `ldaps://` scheme.\
\
**StartTLS**\
\
StartTLS can wrap an unencrypted connection with TLS during the connection process. This allows the same port to handle both unencrypted and encrypted connections. StartTLS uses the `ldap://` scheme.

For either encryption method, if the LDAP server certificate is signed by a private Certificate Authority (CA) or not trusted by the default Java truststore, you must create a custom truststore file or modify a copy of the default Java truststore file to add the CA to the file. Then configure BigConnect Pipelines to use the modified truststore file.

Use the same procedure to configure either secure method.

1. In the $SDC\_CONF/ldap-login.conf file, set either the **useLdaps** or **useStartTLS** property to true. \
   \
   By default, both properties are false and so BigConnect Pipelines makes unencrypted connections to the LDAP server. If you set both properties to true, useStartTLS takes precedence.\

2. Set the **port** property in the ldap-login.conf file as required, based on the method that you enabled:
   * useLdaps - Use the port number for secure connections, typically 636.
   * useStartTLS - Use the LDAP port number, typically 389.\

3.  If the LDAP server certificate is signed by a private CA or not trusted by the default Java truststore, create a custom truststore file or modify a copy of the default Java truststore file to add the CA to the file. Then configure BigConnect Pipelines to use the modified truststore file.\


    By default, BigConnect Pipelines uses the Java truststore file located in $JAVA\_HOME/jre/lib/security/cacerts. If your certificate is signed by a CA that is included in the default Java truststore file, you do not need to create a truststore file and can skip this step.\


    In these steps, we show how to modify the default truststore file to add an additional CA to the list of trusted CAs. If you prefer to create a custom truststore file, see the keytool documentation.\
    \
    **Note**: If you've already configured BigConnect Pipelines to use a custom truststore file to enable HTTPS, then simply add this additional CA to the same modified truststore file that you created in Step 2. Create a Truststore File.\


    1.  Use the following command to set the JAVA\_HOME environment variable:

        ```
        export JAVA_HOME=<Java home directory>
        ```
    2.  Use the following command to set the SDC\_CONF environment variable:

        ```
        export SDC_CONF=<Data Collector configuration directory>
        ```

        For example, for an RPM installation use:

        ```
        export SDC_CONF=/etc/sdc
        ```
    3.  Use the following command to copy the default Java truststore file to the BigConnect Pipelines configuration directory:

        ```
        cp "${JAVA_HOME}/jre/lib/security/cacerts" "${SDC_CONF}/truststore.jks"
        ```
    4.  Use the following keytool command to import the CA certificate into the truststore file:

        ```
        keytool -import -file <LDAP certificate> -trustcacerts -noprompt -alias <LDAP alias> -storepass <password> -keystore "${SDC_CONF}/truststore.jks"
        ```

        \
        For example:

        ```
        keytool -import -file  myLDAPServer.pem -trustcacerts -noprompt -alias MyLDAPServer -storepass changeit -keystore "${SDC_CONF}/truststore.jks"
        ```
    5.  Define the following options in the SDC\_JAVA\_OPTS environment variable:

        * javax.net.ssl.trustStore - Path to the truststore file on the BigConnect Pipelines machine.
        * javax.net.ssl.trustStorePassword - Truststore password.

        Modify environment variables using the method required by your installation type.\


        For example, define the options as follows:

        ```
        export SDC_JAVA_OPTS="${SDC_JAVA_OPTS} -Djavax.net.ssl.trustStore=/etc/sdc/truststore.jks -Djavax.net.ssl.trustStorePassword=mypassword -Xmx1024m -Xms1024m -server -XX:-OmitStackTraceInFastThrow"
        ```

        \
        Or to avoid saving the password in the export command, save the password in a text file and then define the truststore password option as follows: -Djavax.net.ssl.trustStorePassword=$(cat passwordfile.txt)

        \
        Then ensure that the password file is readable only by the user executing the export command.
4. Restart BigConnect Pipelines to enable the changes.

#### Step 3. Map LDAP Groups to BigConnect Pipelines Roles <a href="#ariaid-title7" id="ariaid-title7"></a>

BigConnect Pipelines roles determine the tasks that a user can perform. You map LDAP groups to BigConnect Pipelines roles. An authenticated user account that belongs to that LDAP group can complete the tasks determined by the mapped role.

After you map LDAP groups to BigConnect Pipelines roles, you can assign pipeline permissions to the groups. Pipeline permissions determine the pipeline access that each user has. For example, say you have an LDAP Developer group for all pipeline developers. When you configure the BigConnect Pipelines LDAP properties, you assign the Creator role to the Developer group so they can create new pipelines. To allow the group to edit existing pipelines, you configure the permissions for each pipeline and assign read and write permission to the Developer group. For more information, see Roles and Permissions.

To map LDAP groups to BigConnect Pipelines roles, in the BigConnect Pipelines configuration file, `$SDC_CONF/sdc.properties`, configure the **http.authentication.ldap.role.mapping** property.

BigConnect Pipelines provides the following roles:

| Role    | Description                                                                                                                                                                                                                                                                                    |
| ------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| admin   | Perform any BigConnect Pipelines task. Can perform all tasks listed below, as well as activate BigConnect Pipelines, restart and shut down BigConnect Pipelines, and view BigConnect Pipelines metrics. Enable Control Hub. Install libraries using Package Manager. Generate support bundles. |
| manager | Start and stop pipelines, monitor pipelines, configure and reset alerts. Take, review, and manage snapshots.                                                                                                                                                                                   |
| creator | Create and configure pipelines and alerts, preview data, and monitor the pipeline. Import pipelines.                                                                                                                                                                                           |
| guest   | View pipelines and alerts, and general monitoring information. Export a pipeline.                                                                                                                                                                                                              |

You can map multiple roles to the same group or vice versa. Use a semicolon to separate LDAP groups and commas to separate BigConnect Pipelines roles, as follows:

```
<ldap group>:<SDC role>,<additional SDC role>,<additional SDC role>);<ldap group>:<SDC role>,<additional SDC role>... 
```

When you have finished mapping LDAP groups to roles, restart BigConnect Pipelines to enable the changes to the configuration file.

The following example maps the DEV LDAP group to the creator role, the OPS LDAP group to the manager role, and the SUPER LDAP group to both creator and manager:

```
DEV:creator;OPS:manager;SUPER:creator,manager
```

#### Step 4. Configure Multiple LDAP Servers (Optional) <a href="#ariaid-title8" id="ariaid-title8"></a>

If your organization has multiple LDAP servers, you can configure BigConnect Pipelines to connect to each of the servers.

The steps that you complete to configure multiple LDAP servers depend on the following installation types:

**Installation from the tarball, RPM package, or Docker Hub**\
\
To configure multiple LDAP servers, simply configure the connection information for the additional LDAP servers in the $SDC\_CONF/ldap-login.conf. Then, restart BigConnect Pipelines to enable the changes.\
\
**Cloudera Manager installation**\
\
For a Cloudera Manager installation, configure the connection information for the additional LDAP servers in an advanced configuration snippet - or safety valve - within Cloudera Manager.\
\
In Cloudera Manager, select the StreamSets service, then click **Configuration**. Select **Use Safety Valve to Edit LDAP Information**, and then configure the connection information for all of the LDAP servers in the **BigConnect Pipelines Advanced Configuration Snippet for ldap.login.conf** safety valve.\
\
When you configure multiple LDAP servers in the safety valve, Cloudera Manager ignores all values entered for the `ldap.*` properties in the **Configuration** tab.

Use the following guidelines when configuring multiple LDAP servers:

* The LDAP user account used to log in to BigConnect Pipelines must be registered in at least one of the configured LDAP servers to be authenticated.
* If the LDAP user accounts belong to different LDAP groups in each LDAP server, include all of the group names when you map LDAP groups to BigConnect Pipelines roles in the _$SDC\_CONF/sdc.properties_ file.
* If the additional LDAP servers use different passwords for bindDN - the root distinguished name (DN) for the connection - then directly define the passwords in the bindPassword property.

**Note**: When you configure multiple LDAP servers, BigConnect Pipelines attempts to connect to each server in the order listed in the `ldap-login.conf` file. If BigConnect Pipelines successfully authenticates the user account in one of the LDAP servers, BigConnect Pipelines still continues to authenticate with the remaining LDAP servers. This can cause the BigConnect Pipelines log file to include login failure error messages. You can ignore these error messages.

The following example shows an _`ldap-login.conf`_ file configured to connect to two OpenLDAP servers, server1 and server2. Each server uses the same password for bindDN:

```
ldap {
  com.streamsets.datacollector.http.LdapLoginModule required
  debug="true"
  useLdaps="false"
  useStartTLS="false"
  contextFactory="com.sun.jndi.ldap.LdapCtxFactory"
  hostname="server1" 
  port="389"                 
  bindDn="*******"
  bindPassword="@ldap-bind-password.txt@"  
  forceBindingLogin="true"
  userBaseDn="ou=People,dc=example,dc=org"
  userIdAttribute="uid"
  userPasswordAttribute="userPassword"
  userObjectClass="inetOrgPerson"
  userFilter="uid={user}"
  roleBaseDn="ou=Groups,dc=example,dc=org"
  roleNameAttribute="cn"
  roleMemberAttribute="member"
  roleObjectClass="groupOfNames"
  roleFilter="member={dn}";


  com.streamsets.datacollector.http.LdapLoginModule required
  debug="true"
  useLdaps="false"
  useStartTLS="false"
  contextFactory="com.sun.jndi.ldap.LdapCtxFactory"
  hostname="server2" 
  port="389"                
  bindDn="*******"
  bindPassword="@ldap-bind-password.txt@"  
  forceBindingLogin="true"
  userBaseDn="ou=People,dc=example,dc=org"
  userIdAttribute="uid"
  userPasswordAttribute="userPassword"
  userObjectClass="inetOrgPerson"
  userFilter="uid={user}"
  roleBaseDn="ou=Groups,dc=example,dc=org"
  roleNameAttribute="cn"
  roleMemberAttribute="member"
  roleObjectClass="groupOfNames"
  roleFilter="member={dn}";
};
```

#### Step 5. Enable LDAP Authentication for MapR Stages <a href="#ariaid-title9" id="ariaid-title9"></a>

To use MapR stages with BigConnect Pipelines configured to use LDAP authentication, you must perform an additional step after configuring LDAP authentication.

The MapR distribution for Hadoop uses the Java Authentication and Authorization Service (JAAS) to control security features. The `$MAPR_HOME/conf/mapr.login.conf` file specifies configuration parameters for JAAS.

BigConnect Pipelines expects LDAP configuration to be in the JAAS configuration file `$SDC_CONF/ldap-login.conf` and overrides the `java.security.auth.login.config` system property to point to this file.

To avoid this conflict, copy the contents of the `$MAPR_HOME/conf/mapr.login.conf` file into the `$SDC_CONF/ldap-login.conf` file after you configure the LDAP connection information in the `ldap-login.conf` file.

For example, use the following command to append the contents of the `mapr.login.conf` file to the end of the `ldap-login.conf` file:

```
cat $MAPR_HOME/conf/mapr.login.conf >> $SDC_CONF/ldap-login.conf
```

### &#x20;<a href="#ariaid-title10" id="ariaid-title10"></a>
