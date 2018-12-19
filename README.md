sshm - sshfs mount
==================

A simplistic approach to managing `sshfs` mounts.

You may prefer to use [autosshfs](https://gitlab.com/hellekin/autosshfs). 

This script uses a configuration file at `$HOME/.ssh/sshm_config` to define sshfs mount points. 
Then you can easily mount/unmount/query status of the mounts using the `sshm` script.


### Getting Started

- Pre-requisite: Ensure you have a working version of [sshfs](https://github.com/libfuse/sshfs) installed.
- Clone this repository. 
- Add the `sshm` script to your path.
- Setup the `sshm_config` file.


### Usage

#### sshm_config

Create a `$HOME/.ssh/sshm_config` file with your defined sshfs mounts.
- ideally, you would setup keybased SSH access to the remotes, and use an SSH agent to manage your SSH keys

You can override the location of this file via the `$SSHM_CONFIG_FILE` environment variable.

The sshm_config file is a list of entries of form:

    user@host:path mountpath <sshfs options...>

Entries can be commented out using the standard '#' prefix. Blank lines are ignored.

Each entry in `sshm_config` is basically passed directly in to `sshfs`, although there is some parsing of the configuration to pull out the hostnames.
Because it uses sshfs directly, server definitions in `~/.ssh/config` can be used as for other ssh services - i.e. you use the short form for any services defined in the `~/.ssh/config` file in the host portion of the `sshm_config`.


### Example Usage

See `-h/--help` for detailed usage.

Mount/unmount:

    sshm -m myhost  # mount 'myhost', as defined in $SSHM_CONFIG_FILE
    sshm -u myhost  # unmount 'myhost', as defined in $SSHM_CONFIG_FILE
    sshm -q         # shows mounted hosts
    sshm -a         # shows all hosts


### Configuration

The following environment variables are provided, along with their default value. 
You can set any of them to override their default:

    SSHM_CONFIG_FILE="$HOME/.ssh/sshm_config"  # Path to the sshm_config file
    SSHM_BASE_MOUNTDIR="$HOME/mnt"             # Base directory of where local mounts are created
                                               # Each server is mounted under: $SSHM_BASE_MOUNTDIR/<hostname>
    SSHM_RMDIR_ON_UNMOUNT=true                 # Whether the local mount directory is removed on unmount
                                               # You will likely want to change this to 'false' if mounting under /mnt
    SSHM_MOUNT_OPTIONS="-o reconnect,ServerAliveInterval=15,ServerAliveCountMax=3"  # default sshfs mount options

