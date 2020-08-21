# SSH

## Create a key pair
```
ssh-keygen -t rsa -b 4096
```

## Save to SSH config
[Original source](https://www.freecodecamp.org/news/how-to-manage-multiple-ssh-keys/)
```dotnetcli
# create the config file
nano ~/.ssh/config
```
```
# add config sections for each site with location of key
Host mygithub
  HostName github.com
  User git
  IdentityFile ~/.ssh/_id_rsa
  IdentitiesOnly yes
```
```dotnetcli
# add key password to macos keychain
ssh-add -K ~/.ssh/id_rsa

# ensure ssh-agent is devoid of previous keys
ssh-add -l
ssh-add -D
```
```
# make sure config states passwords are in keychain
Host *
  AddKeysToAgent yes
  UseKeychain yes
```

## Cache credentials locally

```
eval `ssh-agent -s`
ssh-add
```

## List local cached credentials

```
ssh-add -l
```

[user help](https://www.lifewire.com/create-users-useradd-command-3572157)
## Create user
```
useradd kam
```

## Create password for user
```
passwd kam
```

## Add key to remote machine
```
ssh-copy-id -i ~/.ssh/id_rsa.pub klagan@000.111.222.333 -p 22
```
> [source](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/use-remote-desktop)

## Upload SSH key into Azure key vault
```
az keyvault secret set --vault-name my-vault --name my-secret-key-name --file ~/desktop/id_rsa
```

## Download SSH key from Azure key vault
```
az keyvault secret download --vault-name my-vault --name my-secret-key-name --file ~/desktop/my-file
```

install docker and then add user to docker group on build box
```
 sudo usermod -a -G docker klagan
```

## Copy file from client to remote

[Source](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/copy-files-to-linux-vm-using-scp)

Upload from client to ssh remote: `scp kam.txt azureuser@azurehost:uploads`

Download from ssh remote to client: `scp azureuser@azurehost:uploads/file targetfile`

## Install Java

[Source](https://linux4one.com/how-to-install-java-on-ubuntu-18-04/#5_Setting_up_default_Java_Version)

## Shutdown/Reboot

[Source](https://www.lifewire.com/reboot-linux-using-command-line-4032621)

`sudo shutdown --reboot now`

## CLI WIFI configuration

Be sure to install `network manager` package. Try running, `nmcli c`.  If the package is installed it will list the saved WIFI connections.  If it doesnt exist it will prompt the package manager command to install.

`nmtui` brings up a nice CLI GUI to manage the WIFI connections

---

## Troubleshooting

```
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@         WARNING: UNPROTECTED PRIVATE KEY FILE!          @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
Permissions 0666 for '/Users/someone/.ssh/key-file.pem' are too open.
It is required that your private key files are NOT accessible by others.
This private key will be ignored.
Load key "/Users/someone/.ssh/key-file.pem": bad permissions
```

[Source](https://www.howtogeek.com/168119/fixing-warning-unprotected-private-key-file-on-linux/)

As the message says, the file permissions are too open.

```bash
sudo chmod 600 ~/.ssh/my_key.pem
sudo chmod 644 ~/.ssh/known_hosts
sudo chmod 755 ~/.ssh
```

[[linux.md]]
