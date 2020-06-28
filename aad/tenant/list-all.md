# List all

## ..service principals I own

```
az ad sp list --show-mine --query "[].{id:appId, tenant:appOwnerTenantId, name: appDisplayName}"
```

[[service-principals.md]]

