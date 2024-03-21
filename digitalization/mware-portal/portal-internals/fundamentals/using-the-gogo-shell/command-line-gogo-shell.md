# Command Line Gogo Shell

If you’re in a development environment, you can interact with the module framework locally from the command line.

WARNING

Gogo shell should only be run from the command line in development environments. In production environments, run Gogo shell in the Control Panel, as demonstrated in Using the Gogo Shell .

### Prerequisites <a href="#prerequisites" id="prerequisites"></a>

Gogo shell on the command line requires that the server is running in Developer Mode. You can enable Developer Mode on the server in Developer Studio or by setting the following Portal Property:

```properties
include-and-override=portal-developer.properties
```

### Executing Commands <a href="#executing-commands" id="executing-commands"></a>

You can execute Gogo shell commands using Blade CLI or a telnet session.

#### Using Blade CLI <a href="#using-blade-cli" id="using-blade-cli"></a>

You can execute individual commands using Blade CLI:

```bash
blade sh [Gogo shell command]
blade sh [another Gogo shell command]
...
```

#### Using a Telnet Session <a href="#using-a-telnet-session" id="using-a-telnet-session"></a>

You can execute commands in a `telnet` session:

1.  Open a session:

    ```bash
    telnet localhost 11311
    ```
2. Execute Gogo shell commands.
3. When you’re done executing commands, execute the `disconnect` command to end the session.

WARNING

DO NOT execute the following commands. They stop the module framework.

`close`

`exit`

`shutdown`

### Conclusion <a href="#conclusion" id="conclusion"></a>

Now that you know how to run Gogo shell from the command line, explore the available Gogo shell commands. If you need to use Gogo shell in a production environment, please see Using the Gogo Shell.
