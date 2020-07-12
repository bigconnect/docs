# Data Formats

Data formats - such as Avro, JSON, and log - are methods to encode data that adhere to generally accepted specifications.

The way that stages process data can be similar based on the stage type and the type of data being processed. For example, file-based origins such as Directory and SFTP/FTP/FTPS Client will typically process data formats the same way. Similarly, message-based destinations such as Kafka Producer and JMS Producer generally process data formats the same way.

This chapter discusses how stages process data formats generally. For the details of how a stage processes different data formats, see the "Data Formats" section of the stage documentation.

## File Compression Formats

Origins and processors that read files can read uncompressed files, compressed files, archives, and compressed archives.

Hadoop FS reads compressed files automatically. For other origins and processors that read files, you configure the compression format.The following table lists the supported file types by extension:

<table>
  <thead>
    <tr>
      <th style="text-align:left">Compression Format</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">Uncompressed</td>
      <td style="text-align:left">Processes uncompressed files of the configured data format.</td>
    </tr>
    <tr>
      <td style="text-align:left">Compressed</td>
      <td style="text-align:left">
        <p>Processes files compressed by the following compression formats:</p>
        <ul>
          <li>gzip</li>
          <li>bgzip2</li>
          <li>xz</li>
          <li>lzma</li>
          <li>Pack200</li>
          <li>DEFLATE</li>
          <li>Z</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Archive</td>
      <td style="text-align:left">
        <p>Processes files archived by the following archive formats:</p>
        <ul>
          <li>7z</li>
          <li>ar</li>
          <li>arj</li>
          <li>cpio</li>
          <li>dump</li>
          <li>tar</li>
          <li>zip</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Compressed Archive</td>
      <td style="text-align:left">Processes files in compressed archives created by supported compression
        and archive formats.</td>
    </tr>
  </tbody>
</table>

