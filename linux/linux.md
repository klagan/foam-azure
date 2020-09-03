# Linux

**get linux distro/version**

`cat /etc/*-release`

**add to environment path**

[Source](https://www.cyberciti.biz/faq/appleosx-bash-unix-change-set-path-environment-variable/)

**option 1**

```
edit .bash_profile/.bashrc and add this line where '~/.kam' is the new folder to add to path 
```

**option 2**

```
export PATH=$PATH:~/.kam
```

## Install Java

[Source](https://linux4one.com/how-to-install-java-on-ubuntu-18-04/#5_Setting_up_default_Java_Version)


## Shutdown/Reboot

[Source](https://www.lifewire.com/reboot-linux-using-command-line-4032621)

`sudo shutdown --reboot now`

## CLI WIFI configuration

[Source](https://askubuntu.com/questions/461825/how-to-connect-to-wifi-from-the-command-line#461831)

Be sure to `sudo apt install network-manager` package. Try running, `nmcli c` to view current network configuration.  If the package is installed it will list the saved WIFI connections.  If it doesnt exist it will prompt the package manager command to install.

`nmtu` brings up a nice CLI GUI to manage the WIFI connections

## List WIFI 

```
sudo iwlist wlan0 scan
```

## change item owner

[Source](https://medium.com/codebase/unable-to-obtain-lock-file-access-dotnet-cli-3bd313a5009c)

```
sudo chown -R klagan:klagan ~/.nuget
```

## get machine information

```
sudo lshw
```

> [source](https://vitux.com/get-linux-system-and-hardware-details-on-the-command-line/)


[[linux.md]]


