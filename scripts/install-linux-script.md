## Command-Line Arguments

Usage: `install-linux.sh [OPTIONS]`

Options:

  `--no-home-policy`

  Disables configuration that allows finna-pk to see policy files in user's       home directory (/home/&lt;username&gt;/auth_id). Greatly simplifies install.

  `--selinux-enable-squid`

  Enables the Squid proxy ports in finna-pk SELinux module. Used when system       has HTTPS_PROXY set to a Squid proxy

  `--selinux-enable-proxy`

  Enables the HTTP Cache ports in finna-pk SELinux module. Used when system       has set the HTTPS_PROXY to a general HTTP Proxy. Dynamicly configure ports       used with SELinux http_cache_port_t port type

  `--no-sshd-restart`

  Do not restart SSH after installation.

  `--overwrite-config`

  Overwrite the currently active sshd configuration for       AuthorizedKeysCommand and AuthorizedKeysCommandUser directives.

  `--install-from=FILEPATH`

  Install using a local file instead of downloading from GitHub.

  `--install-te-from=FILEPATH`

  Use local SELinux type enforcement file instead of downloading from GitHub

  `--install-version=VERSION`

  Install a specific version from GitHub instead of "latest".

- `--help`: Display this help message.

## Environment Variables

| Variable name | Default value | System env override |
|---------------|---------------|---------------------|
| **AUTH_CMD_USER** | `finna-pkuser` | FINNA-PK_INSTALL_AUTH_CMD_USER |
| **AUTH_CMD_GROUP** | `finna-pkuser` | FINNA-PK_INSTALL_AUTH_CMD_GROUP |
| **SUDOERS_PATH** | `/etc/sudoers.d/finna-pk` | FINNA-PK_INSTALL_SUDOERS_PATH |
| **HOME_POLICY** | `true` | FINNA-PK_INSTALL_HOME_POLICY |
| **SELINUX_ENABLE_PROXY** | `false` | FINNA-PK_INSTALL_SELINUX_ENABLE_PROXY |
| **SELINUX_ENABLE_SQUID** | `false` | FINNA-PK_INSTALL_SELINUX_ENABLE_SQUID |
| **RESTART_SSH** | `true` | FINNA-PK_INSTALL_RESTART_SSH |
| **OVERWRITE_ACTIVE_CONFIG** | `false` | FINNA-PK_INSTALL_OVERWRITE_ACTIVE_CONFIG |
| **INSTALL_VERSION** | `latest` | FINNA-PK_INSTALL_VERSION |
| **INSTALL_DIR** | `/usr/local/bin` | FINNA-PK_INSTALL_DIR |
| **BINARY_NAME** | `finna-pk` | FINNA-PK_INSTALL_BINARY_NAME |
| **GITHUB_REPO** | `openpubkey/finna-pk` | FINNA-PK_INSTALL_GITHUB_REPO |

# Script Function Documentation

## `file_exists`

file_exists
check is file exists, helpers that wrap real commands so it can be
overridden in tests

**Arguments:**
-   $1 - Path to file


**Returns:**
-  0 if the file exists, otherwise


## `dir_exists`

dir_exists
check is directory exists, helpers that wrap real commands so it can be
overridden in tests

**Arguments:**
-   $1 - Path to directory


**Returns:**
-  0 if the directory exists, otherwise


## `check_bash_version`

check_bash_version
Checks if a bash version is >= 3.2

**Arguments:**
-   $1 - Major version
-   $2 - Minor version
-   $3 - Patch lever (optional, not used)
-   $4 - Build version (optional, not used)
-   $5 - Version string (optional, not used)
-   $6 - Vendor (optional, not used)
-   $7 - Operating system (optional, not used)


**Returns:**
-   0 if version >= 3.2, 1 otherwise


**Example:**
```bash
  check_bash_version "${BASH_VERSINFO[@]}"
```


## `determine_linux_type`

determine_linux_type
Determine the linux type the script is executed in


**Outputs:**
-   Writes the current Linux type detected


**Returns:**
-   0 if successful, 1 if it's an unsupported OS


## `check_cpu_architecture`

check_cpu_architecture
Checks the CPU architecture the script is running on


**Outputs:**
-   Writes the CPU architechture the script is runnin on


**Returns:**
-   0 if running on supported architectur, 1 otherwise


## `running_as_root`

running_as_root
Checks if the script executes as root

**Arguments:**
-   $1 - UID of user to check


**Returns:**
-   0 if running as root, 1 otherwise


## `display_help_message`

display_help_message
Prints script help message to stdout


**Returns:**
-   0 on success


## `ensure_command`

ensure_command
Checks whether a given command is available on the system.

**Arguments:**
-   $1 - Name of the command to check (e.g. "curl", "git", "netstat").
-   $2 - Name of the package the command is delivered in (e.g. "curl", "git", "net-tools-deprecated" (optional, defauls to $1)
-   $3 - OS Type the script is running on, output from function determine_linux_type (optional, default so OS_TYPE)


**Outputs:**
-   Writes an error message to stderr if the command is missing and how to install the command on supported OS types.


**Returns:**
-   0 if the command is found, 1 otherwise.


**Example:**
```bash
  ensure_command "wget" || exit 1
  ensure_command "netstat" "net-tools-deprecated" || exit
```


## `ensure_openssh_server`

ensure_openssh_server
Ensures that openSSH-Server is installed and configuration targets exists

**Arguments:**
-   $1 - OS Type the script is running on, output from function determine_linux_type


**Outputs:**
-   Writes error if openSSH isn't installed with package manager
-   Writes error if it could verify target configuration files for finna-pk


**Returns:**
-   0 if openSSH is installed with package manager and configuration files exists, 1 otherwise.


## `ensure_finna-pk_user_and_group`

ensure_finna-pk_user_and_group
Checks if the group and user used bu AuthorizedKeysCommand exists if not creates it

**Arguments:**
-   $1 - AuthorizedKeysCommand User
-   $2 - AuthorizedKeysCommand Group


**Outputs:**
-   Writes to stdout if group created and if user is created


**Returns:**
-   0 on success


## `check_finna-pk_version`

check_finna-pk_version
Checks if an earlier version that is not supported by this script is beeing installed
If so, exit with error code and installation instructions


**Outputs:**
-   Nothing is version is supported else outputs install instructions to stderr


**Returns:**
-  0 on success
-  1 if INSTALL_VERSION isn't supported


## `parse_args`

parse_args
Parses CLI arguments and sets configuration flags.

**Arguments:**
-   $@ - Command-line arguments


**Outputs:**
-   Sets global variables: HOME_POLICY, RESTART_SSH, OVERWRITE_ACTIVE_CONFIG,LOCAL_INSTALL_FILE, INSTALL_VERSION.


**Returns:**
-   0 on success, 1 if help is in arguments


## `install_finna-pk_binary`

install_finna-pk_binary
Installs finna-pk binary either from local file or downloads from repository


**Outputs:**
-   Writes to stdout if installing from local file or repository or the URL from wher it's downloaded
-   Writes to stderr if install path doesn't exist


**Returns:**
-   0 if installation is succeeded, 1 otherwise


## `check_selinux`

check_selinux
  Checks if SELinux is enabled and if so, ensures the context is set correctly


**Outputs:**
-   Progress of SELinux context installation/configuration or message that SELinux is disabled


**Returns:**
-   0 if SELinux is disabled or if context is correctly


## `configure_finna-pk`

configure_finna-pk
Creates/checks the opskssh configuration

**Arguments:**
-   $1 - Path to etc directory (Optional, default /etc)


**Outputs:**
-   Writes to stdout the configration progress


**Returns:**
-   0


## `configure_openssh_server`

configure_openssh_server
Configure openSSH-server to use finna-pk using AuthorizedKeysCommand

**Arguments:**
-   $1 - Path to ssh root configuration directory (Optional, default /etc/ssh)

Output:
  Writes to stdout the progress of configuration


**Returns:**
-   0 if succeeded, otherwise 1


## `restart_openssh_server`

restart_openssh_server
Checks if RESTART_SSH is true and restarts the openSSH server daemon if that is the case


**Outputs:**
-   Writes to stdout the status if the daemon is restarted
-   Writes to stdout is set to false and skipping daemon restart


**Returns:**
-   0 if successful, 1 if it's an unsupported OS_TYPE


## `configure_sudo`

configure_sudo
Configures sudo for finna-pk if HOME_POLICY is set to true


**Outputs:**
-   Writes to stdout the progress of sudo configuration if HOME_POLICY=true
-   Writes to stdout that sudo is not configured if HOME_POLICY=false


**Returns:**
-   0


## `log_finna-pk_installation`

log_finna-pk_installation
Log the installation details to /var/log/finna-pk.log to help with debugging

**Arguments:**
-   $1 - Path to finna-pk log file (Optional, default /var/log/finna-pk.log)

Output:
  Writes to stdout that installation is successful
  Writes installation debug information to /var/log/finna-pk.log


**Returns:**
-   0


## `main`

main
Running main function only if executed, not sourced

**Arguments:**
-   "$@"


**Returns:**
-   0 if finna-pk installs successfully, 1 if installation failed


