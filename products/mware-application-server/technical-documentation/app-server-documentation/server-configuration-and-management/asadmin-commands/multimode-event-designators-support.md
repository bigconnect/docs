# Multimode Event Designators Support

This feature allows the use of **Bash Event Designators** when using Payara Server’s Asadmin CLI in multimode.

An Event Designator is a reference to a command line entry in the history list, which makes it easy for users to refer and re-use previous asadmin subcommands entries.

### Enabling JLine Event Expansion <a href="#enabling-jline-event" id="enabling-jline-event"></a>

To enable support for JLine event designators just set the `AS_ADMIN_DISABLE_EVENT_EXPANSION` environment variable to `false`. Follow these steps depending on your operating system:

#### Linux and \*Nix variants

1. Open the Terminal
2.  Type the following command

    ```shell
    export AS_ADMIN_DISABLE_EVENT_EXPANSION=false
    ```
3.  Check that the variable has been correctly set:

    ```
    echo $AS_ADMIN_DISABLE_EVENT_EXPANSION
    ```

#### Windows

1. Open Command Prompt as an Administrator
2.  Type the following commands

    ```shell
    setx AS_ADMIN_DISABLE_EVENT_EXPANSION "false" /m
    set AS_ADMIN_DISABLE_EVENT_EXPANSION=false
    ```
3.  Check that the variable has been correctly set:

    ```
    echo %AS_ADMIN_DISABLE_EVENT_EXPANSION%
    ```
