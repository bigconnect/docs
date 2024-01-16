# Protobuf Data Format Prerequisites

Perform the following prerequisites before reading or writing protobuf data.

BigConnect Pipelines processes data based on a protobuf descriptor file. The descriptor file (.desc) describes one or more message types. When you configure a stage to process data, you specify the message type to use.

Before processing protobuf data, perform the following tasks:

Generate the protobuf descriptor fileWhen you generate the descriptor file, you need the .proto files that define the message type and any related dependencies.To generate the descriptor file, use the protobuf `protoc` command with the `descriptor_set_out` flag and the .proto files to use, as follows:

```
protoc --include_imports --descriptor_set_out=<filename>.desc <filename>.proto <filename>.proto <filename>.proto...
```

For example, the following command creates an Engineer descriptor file that describes the Engineer message type based on information from the Engineer, Person, and Extension proto files:

```
protoc --include_imports --descriptor_set_out=Engineer.desc Engineer.proto Person.proto Extensions.proto
```

For more information about protobuf and the protoc command, see the protobuf documentation.Store the descriptor file in the BigConnect Pipelines resources directorySave the generated descriptor file in the `$SDC_RESOURCES` directory.&#x20;
