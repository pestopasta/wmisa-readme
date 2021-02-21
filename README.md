## webMethods Integration Server Administration

wmisa is a command line client for administering webMethods Integration Server. Use it to perform operations on administrative resources and execute administrative actions such as restarting the server, installing a package or enabling a cache manager.

## Installation
wmisa requires [Node.js](https://nodejs.org) v 10.15 or later.

    npm install -g @softwareag/wmisa

## Usage

There are three categories of commands.
| Category |Description |
|:--|:--|
|Server Commands  | Perform administrative operations and actions. |
|Help Commands | Discover types of resources, the operations they support and their inputs and outputs. |
|Default Commands | View and modify default values for command line options. |


### Server Commands
Server commands are sent to the Integration Server identified by the `--host` command line option. There are two types of server commands: CRUD commands and action commands.

**CRUD commands** are used to view and modify the configuration of an Integration Server. Not all types of resources support all CRUD operations. For example, the server resource can be used to read high-level information about the server. It cannot be used to create a new server or delete and existing one.

*CRUD Command Formats and Examples* 

    wmisa read <resource-type>
Lists all instances of a type of resource. When combined with the `--expand` option, the details of each resource is displayed. Not all resource types support this operation.
<br>&ensp;&ensp;Example: List all keystore aliases. `wmisa read keystore`
<br>&ensp;&ensp;Example: List details for all keystore aliases. `wmisa read keystore --expand`

    wmisa read <resource-type> <resource-id>
Displays a resource.
<br>&ensp;&ensp;Example: Displays the remote server wmrestoneng01. `wmisa read remoteserver vmrestoneng01`

    wmisa create <resource-type>
Creates a new instance of a resource. Requires a payload defining the state of the resource. You can put the payload a file and use the `--file` option or enter it on the command line with the `--entity` option.
<br>&ensp;&ensp;Example: Create a cache manager named Purchasing from a file.
<br>&ensp;&ensp;&ensp;&ensp;`wmisa create cachemanager Purchasing --file ./newcachemgr.json`
<br>&ensp;&ensp;Example: Create a cache named Orders in the Purchasing cache manager from a file.
<br>&ensp;&ensp;&ensp;&ensp;`wmisa create cachemanager Purchasing cache Orders --file ./newcache.json`

    wmisa update <resource-type> <resource-id>
Updates an existing resource. The payload of changes to make to the resource must be supplied to this command. You can put the payload in a file and use the `--file` option or enter it on the command line with the `--entity` option.
<br>&ensp;&ensp;Example: Update the user btaylor's password. `wmisa update user btaylor --entity "password=S3cre7!"`

    wmisa delete <resource-type> <resource-id>
Deletes a resource.
<br>&ensp;&ensp;Example: Delete the trusted JWT issuer IdProvider01. `wmisa delete jwt issuer IdProvider01`


**Action commands** perform administrative actions on server resources. Each type of resource has its own set of actions. Use the help command to discover them. Most action commands include `name=value` sub-commands. In DOS, these must be surrounded by double quotes.

**Action commands** perform administrative actions on server resources. Each type of resource has its own set of actions. Use the help command to discover them. Most action commands include `name=value` sub-commands. In DOS, these must be surrounded by double quotes.

*Action Command Format and Examples*

    wmisa <admin-action> [sub-command] <resource-type> <resource-id>
Performs the indicated administrative action on a resource.
<br>&ensp;&ensp;Example: Shut down a running Integration Server. `wmisa stop server`
<br>&ensp;&ensp;Example: Restarts an Integration Server immediately, even if there are active sessions. `wmisa restart server "force=true"`
<br>&ensp;&ensp;Example: Install the TestPkg package from Integration Server's replicate/inbound directory but don't activate it.
<br>&ensp;&ensp;&ensp;&ensp;`wmisa install package "filename=TestPkg.zip" "activate=false"`



### Help Commands
Use help commands to discover the types of resources available on an Integration Server, the operations for each type of resource and the inputs and outputs of each operation.

    wmisa help
Lists all types of resources on the Integration Server.

    wmisa help <resource-type>
Lists all the operations that can be performed on the indicated resource.
<br>&ensp;&ensp;Example: Lists all operations that can be performed on a cache manager. `wmisa help cachemanager`

    wmisa help <verb> <resource-type> {<resource-id>}
Lists the inputs and outputs of an operation.
<br>&ensp;&ensp;Example: displays inputs to and outputs from a cache manager operation.  
<br>&ensp;&ensp;&ensp;&ensp;`wmisa help read cachemanager {cacheManagerName}`

You can also get help for the wmisa command line tool.

    wmisa --help
Displays basic usage information.

    wmisa info
Displays extended usage information. 



### Default Commands
wmisa has several command line options. You can save default values for the options so they do not have to be entered on the command line every time. Any option that is supplied on the command line will override its corresponding saved default. The default value is used for any option that is not supplied on the command line.

When referring to an option with a **default** command, use the option's full name, without the dashes. For example, use 'port', not '--port' or '-p'.

 wmisa comes with the following out-of-the-box defaults

    host: localhost:5555
    basicauth: Administrator:manage
 
`wmisa default` lists the defaults for all command line options. Options that do not have a default are not listed.

`wmisa default <option>` displays the default value for a command line option.
<br>&ensp;&ensp;Example: Display the current default value for the --host option. `wmisa default host`

`wmisa default <option> <default-value>` sets a default value for a command line option.
<br>&ensp;&ensp;Example: Change the default for the --host option to is-east001:5589. `wmisa default host is-east001:5589`

`wmisa default reset <option>` resets the default value for a command line option to the factory default.
<br>&ensp;&ensp;Example: Change the default for the --host option to the factory setting, which is localhost:5555. 
<br>&ensp;&ensp;&ensp;&ensp;`wmisa default reset host`

`wmisa default reset` resets the default values for all command line options to the factory defaults.



### Options
The function of `wmisa` is controlled by command-line options. You can view the options with the `wmisa --help` command.
| Option | Description |
|:--|:--|
|-v, --version | display the version number |
|-H, --host \<host:port\> | host and port of the Integration Server instance to connect to (default: localhost:5555)|
|-s, --secure | use HTTPS to connect to Integration Server (default: false) |
|-b, --basicauth \<user:pass\>  | username:password to use when connecting to Integration Server (default: Administrator:manage)|
|-e, --entity \<payload\>  | the request payload |
|-f, --file \<file-name\>  | name of file that holds the request payload |
|-x, --expand | expand lists of resource names to full resources (default: false) |
|-c, --certificate \<cert-file\> | certificate in PEM format to use for HTTPS ports that accept client certificates |
|-k, --key \<key-file\> | private key in PEM format to use for HTTPS ports that accept client certificates |
|-a, --certificateAuthority \<auth-file\>  | certificate chain in PEM format to use to verify the server's certificate<br>used for HTTPS ports; if not specified, server's certificate is not validated |
|-h, --help | display basic usage information  |

Default values for these options can be changed with the `wmisa default` command, described above. Saved defaults can be overridden by specifying the option on the command line. For all options that are not specified on the command line, the default is used.
