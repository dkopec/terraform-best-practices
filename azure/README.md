# Terraform Best Practices - Azure

## Naming Convention

`<resource type>-<business unit>-<service name>-<environment>-<region>-<unique identifier>`

* resource type - [Azure Resource Abbreviation](https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/ready/azure-best-practices/resource-abbreviations)
* business unit - Shorthand identifier of the owning business unit ex. it, cd, etc.
* service name - unique shorthand identifier of the encompassing purpose for the resources, can have multiple dashes to disambiguate. ex. net (for networking), net-vdi, vdi, lz
* environment - the shorthand identifier for the logic deployment environment that the resources are part of ex. prod (production), dev (development), qa, beta, etc
* region - 4 letter abbreviation of Azure region that the resources reside in ex. ncus, wus2 *Note resource groups/containers should use a 3 letter parent regions ex. cus, eus
* unique identifier - if applicable for resources that need to be globally unique, a random assortment of 4 alphanumeric characters.

### Naming Convention Exemptions/Alternations

* When a resource does not allow hyphens drop them

## Resource Specific:

### Keyvaults

- if you're using azurerm_key_vault_access_policy to allow the principle to manipulate the secrets/certs, use the azurerm_key_vault_access_policy.example.key_vault_id to reference the keyvault id on the child resources so that the order of operations can execute properly and destroy properly.
- There is also a provider setting that allows TF to purge keys, which is helpful for building and destroying key vaults while testing
- If you assign a random suffix to the keyvault name in terraform you can avoid issues of it restoring from soft delete and messing up your rollout, while still having the vault there incase you needs its contents. (because terraform will be creating a uniquely named resource each time that random seed is refreshed)

### azurerm_storage_sync_cloud_endpoint
- Rember that its 1 cloud endpoint per azurerm_storage_sync_group
- When using this reasource, set the terraform apply parallelism to 1 to avoid:
  ```
  Status: "Failed"
  Code: "MgmtUnknown"
  Message: "CosmosDB request failed with bad request."
  ```
