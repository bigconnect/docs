# Advanced Options

Pipelines and most pipeline stages include advanced options with default values that should work in most cases. By default, each pipeline and stage hides the advanced options. Advanced options can include individual properties or complete tabs.

As you start designing pipelines, we recommend configuring the basic properties and using the default values for the advanced options. Then as you continue working with pipelines, explore the advanced options to find ways to fine tune processing.

To show advanced options, select **Show Advanced Options** at the top of the properties pane or in a tab.

For example, the following image displays the properties pane for a Directory origin when advanced options are not shown. The three basic properties shown on the General tab are the general properties that you are most likely to use.

![](<../../../.gitbook/assets/image (297).png>)

When you select either Show Advanced Options property, the properties pane shows an additional property on the General tab and shows an additional tab. In the image below, the circled advanced property and tab are configured with recommended default values. You likely won't need to configure these properties until you begin fine tuning how the pipeline processes your data.

![](<../../../.gitbook/assets/image (225).png>)

When appropriate, you can configure BigConnect Pipelines to display advanced options by default. When so configured, all new pipelines – and new stages added to existing pipelines – display advanced options by default.

To show advanced options by default, in the BigConnect Pipelines configuration file, uncomment the `ui.default.configuration.view` property, set it to `ADVANCED`, and restart BigConnect Pipelines.
