# Excel Data Format

You can use file-based origins, such as Directory and Google Cloud Storage, to process Microsoft Excel `.xls` or `.xlsx` files.

When processing Excel files, an origin creates a record for every row in the file. When you configure the origin, you specify whether files include a header row and whether to ignore the header row. A header row must be the first row of a file. Vertical header columns are not recognized.

Origins cannot process Excel files with large numbers of rows. When an origin cannot process a file in the Excel data format, you can save the file as a CSV file in Excel, and then use the delimited data format to process the file.
