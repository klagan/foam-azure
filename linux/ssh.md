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

[[linux.md]]



