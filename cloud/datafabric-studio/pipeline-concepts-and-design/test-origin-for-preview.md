# Test Origin for Preview



A test origin can provide test data for data preview to aid in pipeline development. Test origins are not used when running a pipeline.

A test origin is a virtual stage that you configure in the pipeline or pipeline fragment properties. Test origins do not display in the canvas. This allows you to configure the real origin - the one that you want to use in production - in the canvas, while configuring a test origin in the pipeline or fragment properties.

You can use any available origin as the test origin. The test data can be of any data format allowed by the test origin. The test data does not have to match the data format of the production data.

To use a test origin in data preview, you must select the test origin in the data preview configuration dialog box as the preview source. By default, data preview uses the origin configured in the canvas as the preview source.

For example, say your IT department has provided a separate bucket for test data in Amazon S3. In the canvas, you configure an Amazon S3 origin to use the real bucket for production. In the pipeline properties, you configure a virtual Amazon S3 test origin to use the test bucket.

During pipeline development and testing, you configure data preview to use the test origin, leaving production data untouched. Then when the pipeline is ready, you start the pipeline. Data Collector uses the real Amazon S3 origin to process production data, ignoring the test origin.You can use the test origin as needed, but here are some general scenarios:

* Use a test origin to access test data in the same origin system.

  Use the same type of origin in the canvas and as the test origin, configuring them to access the production data and test data for data preview, respectively.

* Use a test origin to access test data in a different origin system.

  When needed, you can use any origin to access test data for data preview. The original location and data format of the test data becomes irrelevant after it is passed to the pipeline.

* Use a development origin to provide test data.

  If you have some test data readily available, you can use the Raw Data Source origin as the test origin. Or, use other development stages to provide test data.

## Configuring a Test Origin



Configure a test origin in the pipeline properties. When using Control Hub, you can also configure a test origin in the pipeline fragment properties.

1. On the **General** tab of the pipeline or fragment properties, select the origin type that you want to use.You can select any available origin type.
2. On the **Test Origin** tab, configure the origin properties.Origin properties for the test origin are the same as for real origins, with all properties displaying on a single tab.For details about origin configuration, see "Configuring an &lt;origin type&gt; Origin" in the Origins chapter. 

## Using a Test Origin in Data Preview

To use a configured test origin in data preview, configure the preview configuration options.

1. Click the Data Preview icon to start data preview.
2. In the Preview Configuration dialog box, set the Preview Source property to Test Origin, then configure the rest of the data preview properties as needed.For more information about using data preview, see [Data Preview](../data-preview.md) Overview.

