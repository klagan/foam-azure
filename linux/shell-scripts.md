# Shell Scripts

## Begin the script with:
```
#!/bin/bash
```

## Set execute permission on your script: 
```
chmod +x <script>.sh
```

## Execute shell in one of three ways:
```
./<script>.sh
```
```  
sh <script>.sh
```
```
bash <script>.sh
```
## Bash script parameters
![source](https://tecadmin.net/tutorial/bash-scripting/bash-command-arguments/)

### Provide a default of `120000` if the first parameter is null
```
maxWait=${1:-120000}
```

### Display the provided parameters
```
echo $*
```

[[linux.md]]

