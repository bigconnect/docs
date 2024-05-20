# ODBC Driver

Ignite includes an ODBC driver that allows you both to select and to modify data stored in a distributed cache using standard SQL queries and native ODBC API.

For detailed information on ODBC please refer to [ODBC Programmer’s Reference](https://msdn.microsoft.com/en-us/library/ms714177.aspx).

The ODBC driver implements version 3.0 of the ODBC API.

## Cluster Configuration

The ODBC driver is treated as a dynamic library on Windows and a shared object on Linux. An application does not load it directly. Instead, it uses the Driver Manager API that loads and unloads ODBC drivers whenever required.

Internally, the ODBC driver uses TCP to connect to a cluster. The cluster-side connection parameters can be configured via the `IgniteConfiguration.clientConnectorConfiguration` property.

### Thread-Safety

The current implementation of Ignite ODBC driver only provides thread-safety at the connections level. This means that you should not access the same connection from multiple threads without additional synchronization, though you can create separate connections for every thread and use them simultaneously.

### Prerequisites

The Ignite ODBC Driver was officially tested on:

| OS            | <ul><li>Windows (XP and up, both 32-bit and 64-bit versions)</li><li>Windows Server (2008 and up, both 32-bit and 64-bit versions)</li><li>Ubuntu (18.04 64-bit)</li></ul> |
| ------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| C++ compiler  | MS Visual C (10.0 and up), g (4.4.0 and up)                                                                                                                                |
| Visual Studio | 2010 and above                                                                                                                                                             |

### Installing ODBC Driver

In order to use ODBC driver, you need to register it in your system so that your ODBC Driver Manager will be able to locate it.

#### Installing on Windows

For 32-bit Windows, you should use the 32-bit version of the driver. For the 64-bit Windows, you can use the 64-bit driver as well as the 32-bit. You may want to install both 32-bit and 64-bit drivers on 64-bit Windows to be able to use your driver from both 32-bit and 64-bit applications.

**nstalling using installers**

This is the easiest way and one should use it by default. Just launch the installer for the version of the driver that you need and follow the instructions:

32-bit installer: `%IGNITE_HOME%\platforms\cpp\bin\odbc\ignite-odbc-x86.msi` 64-bit installer: `%IGNITE_HOME%\platforms\cpp\bin\odbc\ignite-odbc-amd64.msi`

**Installing manually**

To install ODBC driver on Windows manually, you should first choose a directory on your file system where your driver or drivers will be located. Once you have chosen the location, you have to put your driver there and ensure that all driver dependencies can be resolved as well, i.e., they can be found either in the `%PATH%` or in the same directory where the driver DLL resides.

After that, you have to use one of the install scripts from the following directory `%IGNITE_HOME%/platforms/cpp/odbc/install`. Note, that you may need OS administrator privileges to execute these scripts.

#### Installing on Linux

To be able to build and install ODBC driver on Linux, you need to first install ODBC Driver Manager. The ODBC driver has been tested with [UnixODBC](http://www.unixodbc.org/).

Once you have built the driver and performed the `make install` command, the ODBC Driver i.e. `libignite-odbc.so` will be placed in the `/usr/local/lib` folder. To install it as an ODBC driver in your Driver Manager and be able to use it, perform the following steps:

*   Ensure linker is able to locate all dependencies of the ODBC driver. You can check this by using `ldd` command. Assuming ODBC driver is located under `/usr/local/lib`:

    `ldd /usr/local/lib/libignite-odbc.so`

    If there are unresolved links to other libraries, you may want to add directories with these libraries to the `LD_LIBRARY_PATH`.
* Edit file `${IGNITE_HOME}/platforms/cpp/odbc/install/ignite-odbc-install.ini` and ensure that Driver parameter of the Apache Ignite section points to where `libignite-odbc.so` is located.
* To install the ODBC driver, use the following command:

```
odbcinst -i -d -f ${IGNITE_HOME}/platforms/cpp/odbc/install/ignite-odbc-install.ini
```

To perform this command, you may need root privileges.

Now the Apache Ignite ODBC driver is installed and ready for use. You can connect to it and use it just like any other ODBC driver.
