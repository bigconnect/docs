# Whole File Data Format

You can use the whole file data format to transfer entire files from an origin system to a destination system. With the whole file data format, you can transfer any type of file.

When transferring whole files, Data Collector streams file data from the origin system and writes the data to the destination system based on the directory and file name defined in the destination.

Whole file records contain reference information for the file transfer. They do not contain data from within the files. While you can use several processors to read or modify the reference information, you cannot use typical processors, such as the Field Masker or the Field Replacer, to perform processing on file data. The only processor that substantially alters whole file data is the Whole File Transformer processor, which converts Avro files to Parquet. 

By default, whole file transfers use available resources as needed. You can limit the resources used to transfer whole file data by specifying a transfer rate.

When the origin system provides checksum metadata, you can configure the origin to verify the checksum. Destinations that generate events can include checksum information in event records.

Most destinations allow you to define access permissions for written files. By default, written files use the default permissions of the destination system.

## Basic Pipeline

A pipeline that processes whole files include an origin and one or more destinations that support the whole file data format.

You can include certain processors to read or modify the file reference information included in the record, such as the file name or owner. You can also include a Whole File Transformer processor to convert Avro files to Parquet. However, a basic whole file pipeline looks like this:

![](../../../.gitbook/assets/image%20%28293%29.png)

## Whole File Records

When reading whole files, the origin produces a record with two fields:

* fileref - Contains a reference that enables streaming the file from the origin system to the destination system. You can use scripting processors to read the fileref field. You cannot modify information in the fileref field.
* fileInfo - A map of file properties, such as file path, file name, and file owner. The details differ based on the origin system. You can use processors to modify information in the fileInfo field as needed.

