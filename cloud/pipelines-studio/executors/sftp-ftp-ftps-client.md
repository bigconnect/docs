# SFTP/FTP/FTPS Client

The SFTP/FTP/FTPS Client executor moves or removes a file on an SFTP/FTP/FTPS server each time it receives an event. You cannot perform multiple tasks in the same executor. To perform more than one task, use additional executors.&#x20;

Use the SFTP/FTP/FTPS Client executor as part of an event stream. You can use the executor in any logical way, such as moving a file after receiving a file-processed event from the SFTP/FTP/FTPS Client origin or a file-closure event from the SFTP/FTP/FTPS Client destination.

When you configure the SFTP/FTP/FTPS Client executor, you specify the protocol to use and the URL of the server to connect to.

You specify the task to perform. When deleting a file, you specify the file path of the file to delete. When moving a file, you specify both the file path and the target directory.

If the server requires authentication, configure the credentials for the protocol you are using. For the SFTP protocol, the executor can require that the server be listed in a known hosts file. For the FTPS protocol, the executor can authenticate with the server using a client certificate and can authenticate the certificate from the FTPS server.

For a solution that describes how an executor can move output files after they are written, see Managing Output Files.

### Credentials <a href="#ariaid-title2" id="ariaid-title2"></a>

The SFTP/FTP/FTPS Client executor can use several methods to authenticate with the remote server. From the Credentials tab, configure the authentication required by the remote server.

Authentication options differ for each protocol:

* For all protocols, select an authentication method to log in to the remote server. Choose the method based on the protocol and remote server requirements:
  * None - The stage does not authenticate with the server.
  * Password - The stage authenticates with the server using a user name and password. You must specify the user name and password.
  * Private key - The stage authenticates using a private key. Use only with the SFTP protocol. You must specify the private key, either in a local file or in plain text.
* For the SFTP protocol, the stage can require that the server be listed in a known hosts file. You must specify the path to the known hosts file that contains the host keys for the approved SFTP servers.
* For the FTPS protocol, the stage can use certificates to authenticate with the server. You must specify the keystore file and password. You can also configure the stage to authenticate the server by specifying a truststore provider. For more information about keystores and truststores, see Keystore and Truststore Configuration.
