# Pipeline Types and Icons in Documentation

In BigConnect Pipelines, you can configure pipelines that are run by BigConnect Pipelines and pipelines that are run by BigConnect Pipelines Edge.

In the documentation for each stage, the overview lists the types of pipelines where you can use the stage: BigConnect Pipelines, BigConnect Pipelines Edge, or both. For example, the overview for the Directory origin lists BigConnect Pipelines and BigConnect Pipelines Edge, but the overview for the Amazon S3 origin lists only BigConnect Pipelines.

Within a stage, some features or properties may be limited to particular pipeline types. For example, the Multithreaded Processing documentation for the Directory origin indicates that multithreaded processing is not valid for use in BigConnect Pipelines Edge pipelines.

When there are limitations within a feature, the limitations are stated in the feature documentation. For example, with the HTTP Client origin, batch processing mode is not valid in BigConnect Pipelines Edge pipelines.

Similarly, when a property is not valid for a pipeline type, this limitation is noted in the property description.

When no limitations are mentioned, the documentation for the stage, feature, or property is valid for all pipeline types associated with the parent stage or feature. For example, the First File for Processing documentation for the File Tail origin has no stated limitations because the information is valid for every pipeline type supported by the origin.

The documentation uses the following icons to highlight information specific to a particular pipeline type:

| Pipeline Type Icon                                                                                        | Description                                                 |
| --------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------- |
| ![](https://docs.streamsets.com/datacollector/3.22.x/help/reusable-content/shared-graphics/icon-SDC.png)  | Information related to BigConnect Pipelines pipelines.      |
| ![](https://docs.streamsets.com/datacollector/3.22.x/help/reusable-content/shared-graphics/icon-Edge.png) | Information related to BigConnect Pipelines Edge pipelines. |
