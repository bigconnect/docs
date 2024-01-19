# Protecting Sensitive Data in Configuration Files

You can protect sensitive data in BigConnect Pipelines configuration files by storing the data in an external location and then using the `file` or `exec` function to retrieve the data.

BigConnect Pipelines configuration files include the $SDC\_CONF/sdc.properties file and any additional files included in the BigConnect Pipelines configuration, such as the following files:

* dpm.properties
* vault.properties
* credential-stores.properties

Some configuration file properties, such as the `https.keystore.password` property, require that you enter a password. Instead of entering the password in clear text in the configuration file, you can store the password outside of the configuration file and then use the `file` or `exec` function to retrieve the sensitive data.

You can use functions to retrieve sensitive data in the following ways:\
\
**From a file**\
\
Store the sensitive data in a separate file and then use the `file` function in the configuration file to retrieve the data as follows:

```
${file("<filename>")}
```

For example, if you configure the `xmail.username` property as follows, BigConnect Pipelines retrieves the user name from the email\_username.txt file located in the BigConnect Pipelines configuration directory, `$SDC_CONF`:

```
xmail.username=${file("email_username.txt")}
```

Retrieving sensitive data from another file provides some level of security. However, the sensitive data in the additional file is still entered in clear text and thus vulnerable for others to access. For increased security, use a script or executable to retrieve the sensitive data.\
\
**Using a script or executable**\
\
For increased security, develop a script or executable that retrieves the sensitive data from an external location. For example, you can develop a script that decrypts an encrypted file containing a password. Or you can develop a script that calls an external REST API to retrieve a password from a remote vault system.Use the `exec` function in the configuration file to call the script or executable as follows:

```
${exec("<script name>")} 
```

For example, if you configure the `xmail.password` property as follows, BigConnect Pipelines runs the email\_pwd.sh script to retrieve the password:

```
xmail.password=${exec("email_pwd.sh")}
```

When you use either the `file` or the `exec` function, BigConnect Pipelines uses the exact output of the file or script. So if the output produces a password and then a newline character, BigConnect Pipelines uses the value with the newline character. This causes BigConnect Pipelines to use a password that is not valid. Carefully design and test how you define the output of the file or script to ensure that the functions return only the expected sensitive data.
