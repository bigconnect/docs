# Binary Data Format

When reading binary data, origins generate a record with a single byte array field at the root of the record.

When the data exceeds the user-defined maximum data size, the origin cannot process the data. Because the record is not created, the origin cannot pass the record to the pipeline to be written as an error record. Instead, the origin generates a stage error.

When writing binary data, destinations and processors write binary data to a single field in the record.
