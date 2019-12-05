# Simple and Bulk Edit Mode

Some pipeline and pipeline stage properties include an Add icon \(![](https://streamsets.com/documentation/datacollector/latest/help/datacollector/UserGuide/Graphics/icon_ConfigAddIcon.png)\) where you add additional configurations. You can add the configurations in simple or bulk edit mode. By default, each property uses simple edit mode.

For example, in the Expression Evaluator, you can click the Add icon in simple edit mode to add multiple expressions to the processor. The stage displays text boxes for you to enter configuration values, as follows:

![](../../../.gitbook/assets/image%20%28352%29.png)

Or, you can switch to bulk edit mode to enter a list of configurations in JSON format, for example:

![](../../../.gitbook/assets/image%20%28136%29.png)

When you switch to bulk edit mode, Data Collector displays the correct field names and any values that you entered in simple edit mode. Modify the values, but be sure not to modify the field names. If you modify the field names by mistake, Data Collector displays a validation error message.

In bulk edit mode, you can add additional configurations by entering text in JSON format. Or, you can click the Add icon to add another configuration with empty values and then enter the values in the JSON text.

Bulk edit mode can be particularly useful when you have a large number of configurations to enter. For example, you might want to configure the OPC UA Client origin to subscribe to hundreds of nodes in an OPC UA server. In this case, use the Add icon to add one node configuration. Then, switch to bulk edit mode and copy the JSON text for that node configuration into a text file. Use the same format and required field names to configure additional nodes in the text file. Then simply copy the node configurations from the JSON text file into bulk edit mode, rather than individually adding each node with the Add icon.

