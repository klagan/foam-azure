# Login

```
Install-Module -Name AzureAD -Force 

# should use service principal and not a user principal
$User = "$(my-upn)"
$PWord = ConvertTo-SecureString -String $(my-pwd) -AsPlainText -Force
$Credential = New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $User, $PWord
$response=(Connect-AzureAD -Credential $credential)

# ./run-script
```

[[powershell.md]]

