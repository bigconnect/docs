# Create a Datasource

Follow the instructions given below to create a new Datasource connection in WSO2 Integration Studio.

### Instructions[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/creating-artifacts/data-services/creating-datasources/#instructions) <a href="#instructions" id="instructions"></a>

Follow the steps given below to create the datasource file:

1.  Select the already created [**Datasource Config module**](https://apim.docs.wso2.com/en/4.2.0/integrate/develop/create-integration-project/#datasource-project) in the project navigator, right-click, and go to **New -> Datasource**.

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/data-services/create-datasource.png)

    The **New Datasource** window will open as shown below.

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/data-services/create-datasource-dialog.png)
2. Select your [**datasource config module**](https://apim.docs.wso2.com/en/4.2.0/integrate/develop/create-integration-project/#datasource-project) as the **Container**, add the file name for your datasource, and click **Finish**.

A datasource file will now be created in your datasource config module. Shown below is the sample configuration that is created. You can now update the values in this configuration.

```xml
<datasource>
    <name>MySQLConnection</name>
    <description>MySQL Connection</description>
    <jndiConfig useDataSourceFactory="false">
        <name>MysqlConJNDI1</name>
    </jndiConfig>
    <definition type="RDBMS">
        <configuration>
            <driverClassName>com.mysql.jdbc.Driver</driverClassName>
            <url>jdbc:mysql://localhost:3306/mysqldb</url>
            <username>username</username>
            <password>password</password>
        </configuration>
    </definition>
</datasource>
```
