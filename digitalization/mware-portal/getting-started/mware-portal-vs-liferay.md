# MWARE Portal vs Liferay

MWARE Portal has Liferay DXP at its core as the underlying technology, meaning that all Liferay DXP features are available in MWARE Portal. However, there are some notable changes that MWARE Portal has over Liferay DXP:

* A powerful new [BPM engine](../../mware-bpm/) with support for [BPMN 2.0](../../mware-bpm/bpm-designer/process-editor.md),  [CMMN](../../mware-bpm/bpm-designer/case-editor.md), [Forms](../../mware-bpm/bpm-designer/decision-editor.md), [Decision Tables](../../mware-bpm/bpm-designer/decision-editor.md) and [Workflow Applications](../../mware-bpm/bpm-designer/app-editor.md), Task List and much more
* [Professional, pixel-perfect forms](../process-automation/advanced-forms/) with avanced features like digial signatures, export/import [PDF forms](../process-automation/advanced-forms/form-runner/features/pdf-production/), 1D/2D barcodes, analytics and much more
* Privacy-preserving [traffic monitoring and analytics](../system-administration/monitoring-traffic/)
* Native Single Sign-On integration with [MWARE Identity & Access Management](../../mware-iam.md)
* Seamless deployment and clustering with [MWARE Application Server](../../mware-application-server/)
* Performance tuning and optimizations that enable high concurrency and low latency in handling user requests
* Production binaries released with ahead-of-time (AoT) compilation. These binaries are smaller, start up to 100x faster, provide peak performance with no warmup, and use less memory and CPU than applications running on a Java Virtual Machine (JVM).
* G1 (Garbage-First) garbage collector as the default JVM garbage collector
* Professional support

We release frequently new versions of the portal that follow as close as possible the Liferay versioning system. However, due to the changes and features MWARE Portal brings to the Liferay platform, we need some time to analyze and adapt the features to the new Liferay version, in order to release a product that is stable, tested and enterprise ready.

This documentation uses interchangeably the terms MWARE Portal, the portal, Liferay or DXP to refer to MWARE Portal.
