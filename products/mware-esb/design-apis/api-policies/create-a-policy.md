# Create a Policy

Listed below are two ways of creating a new policy. Depending on whether you need the policy to be accessible only within a specific API or available for all APIs, you can decide on the option you need to follow.

* Creating a **Common Policy**
* Creating an **API Specific Policy**

## Creating a Common Policy <a href="#creating-a-common-policy" id="creating-a-common-policy"></a>

A common policy is a globally maintained policy and you can easily utilize this policy at the operation level of any desired API. Note that these policies are not bound to any API and they are merely templates that you can easily use. Now let’s see how you can create such a policy. Follow the instructions below to create a new common policy.

1.  Sign in to the WSO2 API Publisher.

    `https://<hostname>:9443/publisher`
2.  Navigate to **Policies** tab from the left menu and click **Add New Policy**.

    ![Add policies](https://apim.docs.wso2.com/en/4.2.0/assets/img/design/api-policies/common-policy-1.png)

    Note

    Common policy list will vary depending on the selected `API Gateway`.
3.  Let’s create a policy named **Sample Add Header** for demo purposes. First let’s fill the **General Details** section using the following details.



    | Field            | Sample Value                            |
    | ---------------- | --------------------------------------- |
    | Name             | Sample Add Header                       |
    | Description      | Description of sample add header policy |
    | Applicable Flows | Request, Response                       |

    <figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/design/api-policies/common-policy-2.png" alt=""><figcaption></figcaption></figure>
4.  Next, we move on to the **Gateway Specific Details** section. This is where we upload the policy file that contains the business logic of the policy. The sample policy that we are creating here will be for the `Regular Gateway (Synapse Gateway)`, and hence, the policy should be a `.j2` or `.xml` file. Use the table provided below to fill this section of the form.

    | Field              | Sample Value                                                                                                                                                                                                                                  |
    | ------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Supported Gateways | Regular Gateway                                                                                                                                                                                                                               |
    | Upload Policy File | <p>Copy the below content to a file named <code>sampleAddHeader.j2</code> and upload.</p><pre><code>                &#x3C;property action="set" name="{{headerName}}" value="{{headerValue}}" scope="transport" />
                
    </code></pre> |





    <figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/design/api-policies/common-policy-3.png" alt=""><figcaption></figcaption></figure>
5.  The final section in the form is the **Policy Attributes** section. These attributes are derived from the policy file that you uploaded in the previous step. If you observe the policy content you should notice two dynamic values embedded into that policy, namely the headerName and headerValue. Those are the attributes that we should include when creating our policy (note that you can have a static policy with no attributes if the policy content is static). Let’s fill the **Policy Attributes** section using the following details.

    [![Policy attributes](https://apim.docs.wso2.com/en/4.2.0/assets/img/design/api-policies/common-policy-4.png)](https://apim.docs.wso2.com/en/4.2.0/assets/img/design/api-policies/common-policy-4.png)

    **Attribute 1**

    | Field            | Sample Value                        |
    | ---------------- | ----------------------------------- |
    | Name             | headerName                          |
    | Display Name     | Header Name                         |
    | Required         | True                                |
    | Description      | Name of the header to be added      |
    | Type             | String                              |
    | Validation Regex | ^(\[a-zA-Z\_]\[a-zA-Z\d\_\\-\ ]\*)$ |
    | Default Value    | Leave this blank                    |

    **Attribute 2**

    | Field            | Sample Value                          |
    | ---------------- | ------------------------------------- |
    | Name             | headerValue                           |
    | Display Name     | Header Value                          |
    | Required         | True                                  |
    | Description      | Value of the header                   |
    | Type             | String                                |
    | Validation Regex | ^(\[a-zA-Z\d\_]\[a-zA-Z\d\_\\-\ ]\*)$ |
    | Default Value    | Leave this blank                      |
6.  Once that is done, the completed form should look like the below screenshot. We can now click on **Save**.

    ![Completed policy form](https://apim.docs.wso2.com/en/4.2.0/assets/img/design/api-policies/common-policy-5.png)
7. Now the newly created policy will appear in the table. You can go ahead and search for this policy to verify.
8.  Try viewing this policy by clicking on **View** action. Note that you can download the policy as a `.zip` file using the **Download Policy** button.

    ![Policy view](https://apim.docs.wso2.com/en/4.2.0/assets/img/design/api-policies/common-policy-6.png)

## Creating an API Specific Policy <a href="#creating-an-api-specific-policy" id="creating-an-api-specific-policy"></a>

If you would rather create a policy that is local to the API, you can follow the below provided steps to create such a policy.

1.  Sign in to the WSO2 API Publisher.

    `https://<hostname>:9443/publisher`
2.  Click on any API (e.g., `PizzaShackAPI 1.0.0`) and navigate to the **Policies** tab. You will see a screen like below. Click on the **Add New Policy** button in order to create an API specific policy.

    ![Policy view](https://apim.docs.wso2.com/en/4.2.0/assets/img/design/api-policies/specific-policy-1.png)
3.  Then you will be prompted to enter the policy details. Let’s name this API specific policy as **Local Add Header**. Please refer to steps 3 to 5 from the common policy creation guide above to fill the rest of the form. Fully filled form should look like the below screenshot. Then click on **Save**.

    ![Policy view](https://apim.docs.wso2.com/en/4.2.0/assets/img/design/api-policies/specific-policy-2.png)
4.  You should be able to see a policy named **Local Add Header** under the **Request** tab and **Response** tab of the **Policy List**.

    ![Policy view](https://apim.docs.wso2.com/en/4.2.0/assets/img/design/api-policies/specific-policy-3.png)
5.  Once you hover over the **Local Add Header** policy that you created, you should be able to see the view icon and delete icon as shown in the screenshot below.

    ![Policy view](https://apim.docs.wso2.com/en/4.2.0/assets/img/design/api-policies/specific-policy-4.png)

    * If you click on the **View** action, you will be prompted with a populated policy form with the details you entered.
    * If you choose to delete the policy, you can click on the **Delete** action that appears on policy hover. It will ask for your consent before deleting the API specific policy. On confirmation that policy will be deleted.
