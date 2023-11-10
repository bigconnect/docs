# JSON Threat Protection

The JSON threat protector in WSO2 API Manager validates the request body of the JSON message based on pre-configured to thwart payload attacks.

#### Detecting vulnerabilities before parsing the message[¶](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/api-gateway/threat-protectors/json-threat-protection-for-api-gateway/#detecting-vulnerabilities-before-parsing-the-message) <a href="#detecting-vulnerabilities-before-parsing-the-message" id="detecting-vulnerabilities-before-parsing-the-message"></a>

The json\_validator sequence specifies the properties to be limited in the payload. A sample json\_validator sequence is given below.

```xml
<sequence xmlns="http://ws.apache.org/ns/synapse" name="json_validator">
    <log level="custom">
        <property name="IN_MESSAGE" value="json_validator"/>
    </log>
    <property name="maxPropertyCount"  value = "100"/>
    <property name="maxStringLength"  value = "100"/>
    <property name="maxArrayElementCount"  value = "100"/>
    <property name="maxKeyLength"  value = "100"/>
    <property name="maxJsonDepth"  value = "100"/>
    <property name="RequestMessageBufferSize" value="1024"/>
    <class name="org.wso2.carbon.apimgt.gateway.mediators.JsonSchemaValidator"/>
</sequence>
```

| Property             | Default Value | Description                            |
| -------------------- | ------------- | -------------------------------------- |
| maxPropertyCount     | 100           | Maximum number of properties           |
| maxStringLength      | 100           | Maximum length of string               |
| maxArrayElementCount | 100           | Maximum number of elements in an array |
| maxKeyLength         | 100           | Maximum number length of key           |
| maxJsonDepth         | 100           | Maximum length of JSON                 |

#### Editing the sequence through registry artifacts[¶](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/api-gateway/threat-protectors/json-threat-protection-for-api-gateway/#editing-the-sequence-through-registry-artifacts) <a href="#editing-the-sequence-through-registry-artifacts" id="editing-the-sequence-through-registry-artifacts"></a>

To edit the existing sequence follow the steps below.

1. Log in to the Management Console.
2. Navigate to `/_system/governance/apimgt/customsequences/in/json_validator.xml`
3. Edit the `json_validator.xml` file.
4. Go to the API Publisher and re-publish your API for the changes to take effect.

#### Applying the JSON validator policy[¶](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/api-gateway/threat-protectors/json-threat-protection-for-api-gateway/#applying-the-json-validator-policy) <a href="#applying-the-json-validator-policy" id="applying-the-json-validator-policy"></a>

You can apply the predefined JSON Policy through the UI. Follow the instructions below to apply the json\_validator in sequence.

* Create an API or edit an existing API.
* Go to **Message Mediation** Policies under **Request** configurations of the **Runtime Configurations** tab.
* Select **Edit** the message mediation bar and select **Common Policies** .
*   Select `json\_validator` from the drop-down menu for Common Policies.

    [![Select JSON validator from the drop-down menu](https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/mediation-json-validator.png)](https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/mediation-json-validator.png)
* Scroll down the page and click **Save** to save the changes.

#### Testing the JSON threat protector[¶](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/api-gateway/threat-protectors/json-threat-protection-for-api-gateway/#testing-the-json-threat-protector) <a href="#testing-the-json-threat-protector" id="testing-the-json-threat-protector"></a>

You can edit the sequence to set the property values according to your requirements. A sample request and response for each property value set to 5 is given below.

Note that this exceeds the JSON property count

Request

```java
The request message:
curl -X POST "https://localhost:8243/jsonpolicy/1.0.0/addpayload" -H "accept: application/json" -H "Content-Type: application/json" -H "Authorization: Bearer b227d70b-ca56-3439-8698-ffb90345e1b5" -d "{ \"glossary\": \"value\" \"GlossSee\": \"markup\" }"
```

Response

```xml
<am:fault xmlns:am="http://wso2.org/apimanager">
    <am:code>400</am:code>
    <am:message>Bad Request</am:message>
    <am:description>Request is failed due to JSON schema validation failure:  Max Key Length Reached</am:description>
</am:fault>
```
