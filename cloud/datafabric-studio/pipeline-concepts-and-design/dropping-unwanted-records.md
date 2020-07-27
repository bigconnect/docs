# Dropping Unwanted Records

You can drop records from the pipeline at each stage by defining required fields or preconditions for a record to enter a stage.

## Required Fields

A required field is a field that must exist in a record to allow it into the stage for processing. When a record does not include all required fields, it is processed based on the error handling configured for the pipeline. You can define required fields for any processor, executor, and most destination stages.

Configure required fields as part of the overall pipeline logic or to minimize processing errors. For example, if a Field Hasher encodes social security data in the /SSN field. To ensure all records include social security numbers, you can make /SSN a required field for the stage.

## Preconditions

Preconditions are conditions that a record must satisfy to enter the stage for processing. Stages process all preconditions before passing a record to the stage or to error handling. When a record does not meet all configured preconditions, it is processed based on the error handling configured for the stage.

You can define preconditions for any processor, executor, and most destination stages. 

You can use most functions, pipeline constants, and runtime properties in preconditions.Configure preconditions as part of the overall pipeline logic or to minimize processing errors. 

For example, you might use the following expression to exclude records that originate from outside the United States:

`{record:value('/COUNTRY') == 'US'}`

Error records include information about failed preconditions in the errorMessage record header attribute. You can also view error messages when viewing recent error records in Monitor mode, while the pipeline is running.



