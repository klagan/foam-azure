# Resources

> [Resource provider and types](https://docs.microsoft.com/en-us/azure/azure-resource-manager/management/resource-providers-and-types#azure-cli)

---

### List all resource providers
[(source link)](https://docs.microsoft.com/en-us/azure/azure-resource-manager/management/resource-providers-and-types)

```
az provider list --query "[].{Provider:namespace, Status:registrationState}" --out table
```

### List all resource types

```
az provider show --namespace Microsoft.Storage --query "resourceTypes[*].resourceType" 
```

### List all resources by resource provider/type

```
az resource list --resource-type Microsoft.Storage/storageAccounts
```

### List resources by specific fields (tsv output is not guaranteed order)

```
az resource list --query '[].{name:name, resourceGroup:resourceGroup, location:location, type:type, status:status}' --output tsv
```

---
[[az-cli.md]]
