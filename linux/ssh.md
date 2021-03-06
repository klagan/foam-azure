# SSH

## Create a key pair
```
ssh-keygen -t rsa -b 4096
```

## Save to SSH config
[Source](https://www.freecodecamp.org/news/how-to-manage-multiple-ssh-keys/)
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

## Add key to remote machine
```
ssh-copy-id -i ~/.ssh/id_rsa.pub klagan@000.111.222.333 -p 22
```
> [source](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/use-remote-desktop)

## Copy file from client to remote

[Source](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/copy-files-to-linux-vm-using-scp)

**Upload from client to ssh remote:**

```
scp kam.txt azureuser@azurehost:uploads
```

**Download from ssh remote to client:**

```
scp azureuser@azurehost:uploads/file targetfile
```

**Disable/Enable cleartext password authentication**

```
sudo vi /etc/ssh/sshd_config

<PasswordAuthentication yes/no>

sudo service ssh restart

```

---

## Troubleshooting

### Unprotected private key file

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

### no host keys available - exiting

You can find this error by running `sshd -t`.  This command can reveal errors in the service.

Assuming you have the correct authority:

```
ssh-keygen -A
```

then you can start the service:

```
/etc/init.d/ssh start
```



[[linux.md]]
