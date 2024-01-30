# Implicit and Explicit Validation

Data Pipelines performs two types of validation:

**Implicit validation**

Implicit validation occurs by default as the Data Pipelines Studio saves your changes. Implicit validation lists missing or incomplete configuration, such as an unconnected stage or a required property that has not been configured.

Errors found by implicit validation display in the Issues list. An error icons display on stages with undefined required properties and on the canvas for pipeline issues.

**Explicit validation**

Explicit validation occurs when you click the Validate icon, request data preview, or start the pipeline. Explicit validation becomes available when implicit validation passes.

Explicit validation is a semantic validation that checks all configured values for validity and verifies whether the pipeline can run as configured.

For example, while implicit validation verifies that you entered a value for a URI, explicit validation tests the validity of the URI by connecting to the system.

Errors found by explicit validation display in a list from the validation error message.
