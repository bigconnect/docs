# Step 3. Configure BigConnect Pipelines to Use HTTPS

Modify the BigConnect Pipelines configuration file, sdc.properties, to configure BigConnect Pipelines to use a secure port and your keystore file. If you created a custom truststore file or modified a copy of the default Java truststore file, configure BigConnect Pipelines to use that truststore file.

1.  To define the secure port and keystore file, configure the following properties in the BigConnect Pipelines configuration file, `sdc.properties`:\


    | **Data BigConnect Pipelines HTTPS Property** | **Description**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
    | -------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | sdc.base.http.url                            | <p>BigConnect Pipelines URL. If the property is uncommented and defined, modify to use the HTTPS protocol and the secure port number, for example:</p><p><code>sdc.base.http.url=https://myhost:18636</code></p><p>If the property is commented, you do not need to define it.</p>                                                                                                                                                                                                                                                                                            |
    | https.port                                   | <p>Secure port number for BigConnect Pipelines. For example, 18636.<br></p><p>Any number besides -1 enables the secure port number.<br></p><p><strong>Note</strong>: When both the HTTP and HTTPS port properties are defined in the configuration file, the HTTP port bounces to the HTTPS port.</p>                                                                                                                                                                                                                                                                         |
    | https.keystore.path                          | <p>Path and name of the keystore file. Enter an absolute path or a path relative to the <code>$SDC_CONF</code> directory.<br></p><p>In our example, we'd configure the property as follows:</p><pre><code>https.keystore.path=sdc_company_com.p12
    </code></pre><p><strong>Note</strong>: Default is <code>keystore.jks</code> in the <code>$SDC_CONF</code> directory which provides a self-signed certificate that you can use. However, BigConnect Pipelines strongly recommends that you generate a certificate signed by a trusted CA, as described in Prerequisites.</p> |
    | https.keystore.password                      | <p>Password to open the keystore file. To protect the password, store the password in an external location and then use a function to retrieve the password.<br><br>In our example, we added the password to the default password text file used by BigConnect Pipelines. So we'd simply use the default property value as follows:</p><pre><code>https.keystore.password=${file("keystore-password.txt")}
    </code></pre>                                                                                                                                                      |
2.  For an installation started as a service on operating systems that use the systemd init system, edit the sdc.socket file to use the same secure port that you just defined.The location of the `sdc.socket` file depends on how you installed BigConnect Pipelines:

    * From the RPM package - `/usr/lib/systemd/system/sdc.socket`
    * From the tarball - `/etc/systemd/system/sdc.socket`\


    For example, if you defined the BigConnect Pipelines secure port number as 18636, modify these lines in the file as follows:

    ```
    [Socket]
    ListenStream=18636
    ListenStream=0.0.0.0:18636
    ```
3.  Use the following command to reload the systemd manager configuration:

    ```
    systemctl daemon-reload
    ```
4.  If you created a custom truststore file or modified a copy of the default Java truststore file for BigConnect Pipelines to use, define the following options in the SDC\_JAVA\_OPTS environment variable:

    * javax.net.ssl.trustStore - Path to the truststore file on the BigConnect Pipelines machine.
    * javax.net.ssl.trustStorePassword - Truststore password.

    Modify environment variables using the method required by your installation type.\


    For example, define the options as follows:

    ```
    export SDC_JAVA_OPTS="${SDC_JAVA_OPTS} -Djavax.net.ssl.trustStore=/etc/sdc/truststore.jks -Djavax.net.ssl.trustStorePassword=mypassword -Xmx1024m -Xms1024m -server -XX:-OmitStackTraceInFastThrow"
    ```

    Or to avoid saving the password in the export command, save the password in a text file and then define the truststore password option as follows: -Djavax.net.ssl.trustStorePassword=$(cat passwordfile.txt)

    Then ensure that the password file is readable only by the user executing the export command.
5. Restart BigConnect Pipelines to enable the changes.
