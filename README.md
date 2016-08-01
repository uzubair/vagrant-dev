# Vagrant 

Vagrant is used for builing development enviornments. It improves development/production parity by focusing on automation and lowering development environment setup time.

## Requirements
Make sure you don't have anything listening locally on ports 80 and 8080 before continuing.

### Environments
* [Vagrant](http://www.vagrantup.com/downloads.html) (Mac/Linux: 1.7.4, Windows: [1.7.2](http://www.vagrantup.com/download-archive/v1.7.2.html))
* [Virtual Box](https://www.virtualbox.org/wiki/Download_Old_Builds_4_3)

### Windows Only
* [Cygwin](https://cygwin.com/install.html) with git, openssh, and rsync options
    * *Note*: You may use another terminal with OpenSSH and Rsync as well.
* Disable UAC (if you haven't already)
* You may have to add "C:\Program Files\Oracle\VirtualBox" to your PATH environment variable

## Install DevEnv

### Quick Start

The following commands will setup the minimum requirements. For further information, and an explanation of each step, read the next section.

```bash
vagrant plugin install vagrant-docker-compose    # Install the vagrant-docker-compose plugin
vagrant up                                       # Initialize your environment
vagrant ssh                                      # SSH into the machine
```

### Install the vagrant-docker-compose plugin

```bash
vagrant plugin install vagrant-docker-compose
```

### Initializing your enviornment
> Roughly takes about 10-20 minutes.

```bash
vagrant up
```

### SSH into the machine

```bash
vagrant ssh
```

### Rsync
> To synchronize your files with the virtual machine, run the following command in a seperate terminal window:

```bash
vagrant rsync-auto
```

## Configuration

**Note:** Specify the local machine directory that you want to mount and sync with the virtual machine by updating `line 57` in Vagrantfile
```bash
# Mount directories
mount_folder server, "directory_to_mount", "/home/vagrant/git", true
```