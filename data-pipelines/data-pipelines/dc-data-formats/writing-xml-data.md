# Writing XML Data

When writing XML data, destinations create a valid XML document for each record. The destination requires the record to have a single root field that contains the rest of the record data.When writing XML data, you can configure the destination to perform the following tasks:

* Produce a "pretty" output - The destination can add indentation to make the XML data human-readable. This adds additional bytes to the record size.
* Validate the schema - The destination can validate that the generated XML conforms to the specified schema definition. Records with invalid schemas are handled based on the error handling configured for the destination.

## Record Structure Requirement

When writing XML data, the destination expects all record data under a single root field. When necessary, merge record data into a root field earlier in the pipeline. You can use the Expression Evaluator and Field Remover processors to perform this task.

For example, in the following pipeline, the Expression Evaluator uses the expression, `${record:value('/')}`, to create a root field and copy the entire record under the root field:

![](../../../.gitbook/assets/image%20%28294%29.png)

Then, you can use the Field Remover to keep only the root field, which removes all other fields:

![](../../../.gitbook/assets/image%20%2859%29.png)

