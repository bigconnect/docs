# Generate Service Catalog Metadata Artifacts

Follow the instructions given below to generate Service Catalog metadata artifacts for APIs and Proxy Services in WSO2 Integration Studio older workspaces to expose integration services as Managed APIs.

### Generate Swagger and Metadata files for APIs[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/generate-service-catalog-metadata/#generate-swagger-and-metadata-files-for-apis) <a href="#generate-swagger-and-metadata-files-for-apis" id="generate-swagger-and-metadata-files-for-apis"></a>

1.  Right-click the **api** folder under the ESB project and click **Generate API Metadata**.

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/generate\_metadata/api-metadata.png)
2.  This will open a success dialog box once finished.

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/generate\_metadata/api-success.png)
3.  Re-select required API artifacts under the relevant **Composite Exporter** module dependencies section to pack the generated metadata artifacts along with the API artifacts.

    Tip

    By default, the `Publish to Service Catalog` checkbox is enabled. If not, select the checkbox in the wizard so that it will include metadata files of the selected artifacts.

### Generate Metadata files for Proxy Services[¶](https://apim.docs.wso2.com/en/latest/integrate/develop/generate-service-catalog-metadata/#generate-metadata-files-for-proxy-services) <a href="#generate-metadata-files-for-proxy-services" id="generate-metadata-files-for-proxy-services"></a>

1.  Right-click the **proxy-service** folder under the ESB project and click **Generate API Metadata**.

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/generate\_metadata/proxy-metadata.png)
2.  This will open a success dialog box once finished.

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/integrate/create\_artifacts/generate\_metadata/proxy-success.png)
3. Re-select required Proxy Service artifacts under the relevant **Composite Exporter** module dependencies section to pack the generated metadata artifacts along with the Proxy Service artifacts.
