# Bash

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

## Alias
```
alias ls='ls -ahl --color'
```

## Reload .bashrc
```
source ~/.bashrc
```

## Structures



This sample contains loops, json manipulation and `az` command



**example json parameter**

```

{

"1": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",

"2": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",

"3": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",

"4": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",

}  

```



**example app id parameter**

```

xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx

```



**code snippet**

```

if [ $# -ne 2 ]; then

    echo "ERROR: [Incorrect arguments provided"]

    echo -e "eg: ${0} <json> <application-id>\n"

    exit 1

fi



json=$1

appId=$2



for destAppId in $(echo "${json}" | jq -r '.[] | values'); 

do   

    echo $destAppId

    echo "Provisioning ${appId} permission to ${destAppId} api"

    

    apiPermission=$(az ad app show --id "${destAppId}" --output tsv --query "oauth2Permissions[?value=='user_impersonation'].{id:id}")

    # apiRolePermission=$(az ad app show --id $destAppId --output tsv --query "appRoles[?value=='My.Role'].{id:id}")

   

    az ad app permission delete --id "${appId}" --api "${destAppId}"

    az ad app permission add --id "${appId}" --api "${destAppId}" --api-permissions "${apiPermission}=Scope"

    # az ad app permission add --id "${appId}" --api "${destAppId}" --api-permissions "${apiRolePermission}=Role"

done

echo "##vso[task.logissue type=warning]Must run the following commands manually to grant admin access"

echo "az ad app permission admin-consent --id ${appId}"

```



[[linux.md]]

