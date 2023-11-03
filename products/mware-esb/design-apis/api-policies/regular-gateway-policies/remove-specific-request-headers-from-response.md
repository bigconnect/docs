# Remove Specific Request Headers From Response

Web services may contain response headers with sensitive information. The following explains how to remove HTTP request headers from the responses for security reasons.

To remove request headers from responses for per API or globally, add the name of the header to be removed as a property in your custom `out` sequence.

Example

```bash
<property name="<name of the header to be removed>" scope="transport" action="remove"/>
```



## Handling Error Responses <a href="#handling-error-responses" id="handling-error-responses"></a>

To handle error responses, follow the instructions below.

1. To address the scenario where the API does not exist, open the `<API-M_HOME>/repository/deployment/server/synapse-configs/default/sequences/main.xml` file.
2.  Add the name of the header to be removed as a property, just before the beginning of `send` mediator, as shown below.



    ```bash
    <property name="<name of the header to be removed>" scope="transport" action="remove"/>
    <send/>
    ```

    Sample
3. To address the scenario where an error occurs during execution of API requests, open the `<API-M_HOME>/repository/deployment/server/synapse-configs/default/sequences/fault.xml` file.
4.  Add the name of the header to be removed as a property property, just before the beginning of `CORS Request Handler` sequence, as shown below.



    ```bash
    <property name="<name of the header to be removed>" scope="transport" action="remove"/>
    <sequence key="_cors_request_handler_"/>
    ```

    Sample

\
