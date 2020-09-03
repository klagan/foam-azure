# User management

## Add a user

```
useradd -m klagan
```

## Check defaults for new user

```
useradd -D
```

## Change prompt

[Source](https://www.cyberciti.biz/tips/howto-linux-unix-bash-shell-setup-prompt.html)

## Add groups to user

[Source](https://www.howtogeek.com/50787/add-a-user-to-a-group-or-second-group-on-linux/)

```
sudo usermod -a -G <groups> <username>
```
