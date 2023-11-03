# Customize API Life Cycle

APIs created in WSO2 API Manager have their own life cycle consisting of the following: a set of life cycle states, specific actions for each state transition, and a checklist of items before a state transition occurs. An API has a predefined life cycle consists of [six states](https://apim.docs.wso2.com/en/4.2.0/design/lifecycle-management/api-lifecycle/#api-lifecycle-states). This tutorial demonstrates how you can edit the default API lifecycle and customize it according to your requirements.

Follow the steps below to add a new state to the default life cycle.

1. Sign in to the API Publisher and click on the API that you created previously (e.g., PizzaShackAPI - 1.0.0).
2.  Click **Lifecycle** tab to view the current state and states available by default.&#x20;

    <figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/default-lifecycle.png" alt=""><figcaption></figcaption></figure>
3. Open the API Manager Management Console: `https://localhost:9443/admin`
4.  Click on the Advanced tab.

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/admin-dashboard-advanced.png)
5.  By default LifeCycle configuration doesn't appear in the advanced configurations. To view the default LifeCylce configuration type as **LifeCycle** and select **LifeCycle** autosuggested option.

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/admin-dashboard-lifecycle-config.png)

    The default LifeCycle is given below.

    ```java
    "LifeCycle": {
        "States": [
            {
                "Transitions": [
                    {
                        "Target": "Published",
                        "Event": "Publish"
                    },
                    {
                        "Target": "Prototyped",
                        "Event": "Deploy as a Prototype"
                    }
                ],
                "State": "Created",
                "CheckItems": [
                    "Deprecate old versions after publishing the API",
                    "Requires re-subscription when publishing the API"
                ]
            },
            {
                "Transitions": [
                    {
                        "Target": "Published",
                        "Event": "Publish"
                    },
                    {
                        "Target": "Created",
                        "Event": "Demote to Created"
                    },
                    {
                        "Target": "Prototyped",
                        "Event": "Deploy as a Prototype"
                    }
                ],
                "State": "Prototyped",
                "CheckItems": [
                    "Deprecate old versions after publishing the API",
                    "Requires re-subscription when publishing the API"
                ]
            },
            {
                "Transitions": [
                    {
                        "Target": "Blocked",
                        "Event": "Block"
                    },
                    {
                        "Target": "Prototyped",
                        "Event": "Deploy as a Prototype"
                    },
                    {
                        "Target": "Created",
                        "Event": "Demote to Created"
                    },
                    {
                        "Target": "Deprecated",
                        "Event": "Deprecate"
                    },
                    {
                        "Target": "Published",
                        "Event": "Publish"
                    }
                ],
                "State": "Published"
            },
            {
                "Transitions": [
                    {
                        "Target": "Deprecated",
                        "Event": "Deprecate"
                    },
                    {
                        "Target": "Published",
                        "Event": "Re-Publish"
                    }
                ],
                "State": "Blocked"
            },
            {
                "Transitions": [
                    {
                        "Target": "Retired",
                        "Event": "Retire"
                    }
                ],
                "State": "Deprecated"
            },
            {
                "State": "Retired"
            }
        ]
    }
    ```

    6\. Update the API lifecycle definition.

    1.  Add a sample state to the API Lifecycle. Copy and paste the following sample code in the file.

        ```java
            {
                "State": "Rejected",
                "Transitions": [
                    {
                        "Event": "Re-Submit",
                        "Target": "Published"
                    },
                    {
                        "Event": "Retire",
                        "Target": "Retired"
                    }
                ]
            }
        ```
    2.  Add a new transition event under the PUBLISHED state and under the Transitions, to show the state change to REJECTED.

        ```java
        ...   
        {
            "Event": "Reject",
            "Target": "Rejected"
        }
        ...
        ```
6. Restart the WSO2 API Manager server.
7.  Open API Publisher and check the Lifecycle tab to see the newly added state.

    ![](https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/custom-life-cycle-states.png)
