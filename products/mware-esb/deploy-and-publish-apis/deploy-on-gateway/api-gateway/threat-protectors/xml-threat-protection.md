# XML Threat Protection

The XML threat protector in WSO2 API Manager validates the XML payload vulnerabilities based on the pre-configured limits. It uses following methodologies to thwart the gateway from XML based attacks.

#### Detecting the malformed, vulnerable XML messages through limitations[¶](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/api-gateway/threat-protectors/xml-threat-protection-for-api-gateway/#detecting-the-malformed-vulnerable-xml-messages-through-limitations) <a href="#detecting-the-malformed-vulnerable-xml-messages-through-limitations" id="detecting-the-malformed-vulnerable-xml-messages-through-limitations"></a>

The xml\_validator sequence specifies the properties to be limited in the payload. A sample xml\_validator sequence is given below.

```xml
<sequence xmlns="http://ws.apache.org/ns/synapse" name="xml_validator">
    <log level="custom">
        <property name="IN_MESSAGE" value="xml_validator"/>
    </log>
    <property name="xmlValidation" value="true"/>
    <property name="dtdEnabled" value="false"/>
    <property name="externalEntitiesEnabled" value="true"/>
    <property name="maxXMLDepth" value="100"/>
    <property name="maxElementCount" value="100"/>
    <property name="maxAttributeCount" value="100"/>
    <property name="maxAttributeLength" value="100"/>
    <property name="entityExpansionLimit" value="100"/>
    <property name="maxChildrenPerElement" value="100"/>
    <property name="entityExpansionLimit" value="100"/>
    <property name="schemaValidation" value="true"/>
    <switch source="get-property('To')">
        <case regex=".*/addResource.*">
            <property name="xsdURL" value="<Insert the XSD URL>"/>
        </case>
        <!--<case regex=".*/update.*">-->
            <!--<property name="xsdURL" value="<insert XSD_URL>"/>-->
        <!--</case>-->
        <!--<case regex=".*/delete.*">-->
            <!--<property name="xsdURL" value="<insert XSD_URL>d"/>-->
        <!--</case>-->
    </switch>
    <property name="RequestMessageBufferSize" value="1024"/>
    <class name="org.wso2.carbon.apimgt.gateway.mediators.XMLSchemaValidator"/>
</sequence>
```

Users can enable or disable XML payload limits and schema validation. Some examples are shown below.

Disabling the XML payload validation

```xml
<property name="xmlValidation" value="false"/>
```

Disabling the XML schema validation

**XML payload validation properties**[**¶**](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/api-gateway/threat-protectors/xml-threat-protection-for-api-gateway/#xml-payload-validation-properties)

* Disable the DTD payload in the XML properties to avoid attacks
*   You can turn on/off external entities of the payload. An example is given below with the elements of the XML request body, that can be configured .

    | Property                | Default Value | Description                                                          |
    | ----------------------- | :-----------: | -------------------------------------------------------------------- |
    | dtdEnabled              |     false     | The DTD can be enabled/disabled according to your requirement.       |
    | externalEntitiesEnabled |      true     |                                                                      |
    | maxXMLDepth             |      100      | Maximum depth of the XML request message                             |
    | maxElementCount         |      100      | Maximum number of allowed elements in the XML request message.       |
    | maxAttributeCount       |      100      | Maximum count of allowed attributes in the XML request message.      |
    | maxAttributeLength      |      100      | Maximum allowed length of each attribute value in characters.        |
    | entityExpansionLimit    |      100      | Maximum allowed entity expansion limit of the XML request message.   |
    | maxChildrenPerElement   |      100      | Maximum number of child elements allowed in the XML request message. |

#### XML schema validation[¶](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/api-gateway/threat-protectors/xml-threat-protection-for-api-gateway/#xml-schema-validation) <a href="#xml-schema-validation" id="xml-schema-validation"></a>

You can define XML schemas per resource to validate each request. For example, to add an XML schema to the resource  /userapi/1.0.0/addResource/value follow the steps below.

1. Define the resource in the case regex
2. Define the relevant schema URL and add it as shown below.
3. You can define the buffer size of the request message depending on your requirement. An example is given below.

Each request is sanitized through the XML threat protector. API developer can modify each properties according to your requirement.

#### Editing the sequence through registry artifacts[¶](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/api-gateway/threat-protectors/xml-threat-protection-for-api-gateway/#editing-the-sequence-through-registry-artifacts) <a href="#editing-the-sequence-through-registry-artifacts" id="editing-the-sequence-through-registry-artifacts"></a>

To edit the existing sequence follow the steps below.

1. Log in to the Management Console.
2. Navigate to `/_system/governance/apimgt/customsequences/in/xml_validator.xml`
3. Edit the `xml_validator.xml` file.
4. Go to the API Publisher and re-publish your API for the changes to take effect.

#### Applying the XML validator policy[¶](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/api-gateway/threat-protectors/xml-threat-protection-for-api-gateway/#applying-the-xml-validator-policy) <a href="#applying-the-xml-validator-policy" id="applying-the-xml-validator-policy"></a>

You can apply the predefined XML Policy through the UI. Follow the instructions below to apply the xml\_validator in sequence.

* Create an API or edit an existing API.
* Go to **Message Mediation** Policies under **Request** configurations of the **Runtime Configurations** tab.
* Select **Edit** in the message mediation bar and select **Common Policies** .
*   Select `xml_validator` from the drop-down menu for Common Policies.

    [![Select XML validator from the drop-down menu](https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/mediation-xml-validator.png)](https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/mediation-xml-validator.png)
* Scroll down the page and click **Save** to save the changes.

#### Testing the XML threat protector[¶](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/api-gateway/threat-protectors/xml-threat-protection-for-api-gateway/#testing-the-xml-threat-protector) <a href="#testing-the-xml-threat-protector" id="testing-the-xml-threat-protector"></a>

You can edit the sequence to set the property values according to your requirements. A sample request and response for the value of the properties set to 30 is given below. Note that the .xsd URL for the relevant resource has been hosted.

Request

```java
curl -X POST "https://192.168.8.101:8243/xmlPolicy/1.0.0/addResource" -H "accept: application/json" -H "Content-Type: application/xml" -H "Authorization: Bearer 2901c002-f626-372c-9be3-fc54b2c8d65f" -d "<?xml version=\"1.0\"?><inline_model> <Name>string</Name> <Age>string</Age> <Address>string</Address> <phone>string</phone> <home>string</home> <path>string</path> <class>string</class> <team>string</team> <location>string</location> <brand>string</brand> <summary>string</summary> <data>string</data> <Name>string</Name> <Age>string</Age> <Address>string</Address> <phone>string</phone> <home>string</home> <path>string</path> <class>string</class> <team>string</team> <location>string</location> <brand>string</brand> <summary>string</summary> <data>string</data> <Name>string</Name> <Age>string</Age> <Address>string</Address> <phone>string</phone> <home>string</home> <path>string</path> <class>string</class> <team>string</team> <location>string</location> <brand>string</brand> <summary>string</summary> <data>string</data> <Name>string</Name> <Age>string</Age> <Address>string</Address> <phone>string</phone> <home>string</home> <path>string</path> <class>string</class> <team>string</team> <location>string</location> <brand>string</brand> <summary>string</summary> <data>string</data></inline_model>"
```

Response

#### Testing the schema validation[¶](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/api-gateway/threat-protectors/xml-threat-protection-for-api-gateway/#testing-the-schema-validation) <a href="#testing-the-schema-validation" id="testing-the-schema-validation"></a>

A sample request and response to test the schema validation is given below.

Request

```java
curl -X POST "https://192.168.8.101:8243/xmlPolicy/1.0.0/addResource" -H "accept: application/json" -H "Content-Type: application/xml" -H "Authorization: Bearer 2901c002-f626-372c-9be3-fc54b2c8d65f" -d "<?xml version=\"1.0\"?><inline_model> <location>string</location> <brand>string</brand> <summary>string</summary> <data>string</data> <Name>string</Name> <Age>string</Age> <Address>string</Address> <phone>string</phone> <home>string</home> <path>string</path> <class>string</class> <team>string</team> <location>string</location> <brand>string</brand> <summary>string</summary> <data>string</data> <Name>string</Name> <Age>string</Age> <Address>string</Address> <phone>string</phone> <home>string</home> <path>string</path> <class>string</class> <team>string</team> <location>string</location> <brand>string</brand> <summary>string</summary> <data>string</data></inline_model>"
```
