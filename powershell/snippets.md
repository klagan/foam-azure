# Snippets

Sample snippet exhibiting loops, json and `az`

**example json parameter**
```
{
"1": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
"2": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
"3": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
"4": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",          
} 
```

**example appId parameter**
```
xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```

```
param(
    [parameter(mandatory=$true)][string]$json,
    [parameter(mandatory=$true)][string]$appId
  )

if (([string]::IsNullOrEmpty($json)))
{
    Write-Error "Endpoint json parameter is empty"
    Exit 1
}

if (([string]::IsNullOrEmpty($appId)))
{
    Write-Error "The app id parameter is empty"
    Exit 1
}

# Install-Module -Name AzureAD -Force 

$ids = $json | ConvertFrom-Json

foreach ($id in $ids.PsObject.Properties) {

    $id=$endpoint.Value
    $apiPermission=$(az ad app show --id "$($id)" --output tsv --query "oauth2Permissions[?value=='user_impersonation'].{id:id}")
    az ad app permission delete --id "$($appId)" --api "$($id)"
    az ad app permission add --id "$($appId)" --api "$($id)" --api-permissions "$($apiPermission)=Scope"
}

Write-Host "##vso[task.logissue type=warning]Must run the following commands manually to grant admin access"

Write-Host "az ad app permission admin-consent --id $($appId)"
```

# Useful commands

### Elevate execution policy
```
Set-ExecutionPolicy Bypass -Scope Process
```

## Write-XXX

`Write-Host` directly to the console, not included in function/cmdlet output. Allows foreground and background colour to be set.

`Write-Debug` directly to the console, if $DebugPreference set to Continue or Stop.

`Write-Verbose` Write directly to the console, if $VerbosePreference set to Continue or Stop.

`Write-Information` Uses the `$InformationPreference` flag to determine how to handle the message.  `Write-Host` is a wrapper for this call. [(source)](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/write-information?view=powershell-7#parameters)

### Test network connection

```
nc google.com -port 80
```

### List environment variables
```
gci env:* | sort-object name
```

### Connect to Azure AD
> [source](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.security/get-credential?view=powershell-7#examples)
```
$User = "Domain01\User01"
$PWord = ConvertTo-SecureString -String "P@sSwOrd" -AsPlainText -Force
$Credential = New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $User, $PWord
```
In `Azure Powershell` task
```
 $context = [Microsoft.Azure.Commands.Common.Authentication.Abstractions.AzureRmProfileProvider]::Instance.Profile.DefaultContext
 $graphToken = [Microsoft.Azure.Commands.Common.Authentication.AzureSession]::Instance.AuthenticationFactory.Authenticate($context.Account, $context.Environment, $context.Tenant.Id.ToString(), $null, [Microsoft.Azure.Commands.Common.Authentication.ShowDialog]::Never, $null, "https://graph.microsoft.com").AccessToken
 $aadToken = [Microsoft.Azure.Commands.Common.Authentication.AzureSession]::Instance.AuthenticationFactory.Authenticate($context.Account, $context.Environment, $context.Tenant.Id.ToString(), $null, [Microsoft.Azure.Commands.Common.Authentication.ShowDialog]::Never, $null, "https://graph.windows.net").AccessToken
 Connect-AzureAD -AadAccessToken $aadToken -AccountId $context.Account.Id -TenantId $context.tenant.id
 ```
### Pipe to a file

```
 <your command> | Out-File -FilePath kam.txt
```

### Formatting results

This example *filters* the `$userRoles` list for items with a `ResourceDisplayName` of *my-resource* and then returns the results as a list with the specified properties

```
$userRoles | Where-Object {$_.ResourceDisplayName -eq 'my-resource'} | Format-List -Property ObjectId, ObjectType, CreationTimeStamp, Id, PrincipalDisplayName, PrincipalId, PrincipalType, ResourceId, ResourceDisplayName
 
```


[[powershell.md]]


