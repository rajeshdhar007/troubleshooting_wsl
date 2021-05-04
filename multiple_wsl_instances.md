# For Ubuntu
Download the rootfs from: 
https://cloud-images.ubuntu.com/

https://cloud-images.ubuntu.com/focal/20210503/

https://cloud-images.ubuntu.com/focal/20210503/focal-server-cloudimg-amd64-wsl.rootfs.tar.gz

_filename: 
focal-server-cloudimg-amd64-wsl.rootfs.tar.gz_

## List all WSL Images Installed

```
C:\Users\rdhar> wsl --list --all -v
  NAME                   STATE           VERSION
* Ubuntu                 Running         2
  docker-desktop-data    Running         2
  docker-desktop         Running         2
```

## Set default WSL version version

```
wsl --set-default-version 2
```

## WSL Import the ROOTFS
```
Syntax:
wsl --import <NameOfInstallation> <LocationOnHDD> <LocationOfROOTFS.tar.gz> --version <number>

Example:
wsl --import UbuntuDev20.10 C:\Users\rdhar\Workspace\vms\custom_wsl\UbuntuDev20.10 C:\Users\rdhar\Downloads\OS\focal-server-cloudimg-amd64-wsl.rootfs.tar.gz --version 2
```

## WSL Export an Installation
```
Syntax:
wsl --export <NameOfInstallation> <TargetLocationOnHDD.tar.gz>

Example:
wsl --export Ubuntu-20.04 E:\WSL2\ubuntu.tar
```

## WSL Unregister/Delete an Installation
```
Syntax:
wsl --unregister <NameOfInstallation>

Example:
wsl --unregister Ubuntu-20.04
```

## WSL Terminate an Instance
```
Syntax:
wsl --terminate <NameOfInstallation>

Example:
wsl --terminate Ubuntu-20.04
```

## WSL Launch an Installation
```
Syntax:
wsl -d <NameOfInstallation>

Example:
wsl -d UbuntuDev20.10
```

### Add user
```
adduser <username>
```

### Add user to group
```
usermod -a -G <groupname> <username>
```

### Add default user for WSL
```
echo -e "[user]\ndefault=<username>" >> /etc/wsl.conf
```