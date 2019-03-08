# Apply tag and its default value

## Powershell
### Deploy with Powershell
```
# Create the Policy Definition (Subscription scope)
$definition = New-AzPolicyDefinition -Name 'apply-default-tag-value' -DisplayName 'Apply tag and its default value' -description 'Applies a required tag and its default value if it is not specified by the user' -Policy 'https://raw.githubusercontent.com/alchen99/az-policies/master/apply-default-tag-value/azurepolicy.rules.json' -Parameter 'https://raw.githubusercontent.com/alchen99/az-policies/master/apply-default-tag-value/azurepolicy.parameters.json' -Mode All

# Set the scope to a resource group; may also be a resource, subscription, or management group
# setting Subscription scope
$scope = '/subscriptions/0b1f6471-1bf0-4dda-aec3-111122223333'

# Set the Policy Parameter (JSON format)
$policyparam = '{ "tagName": { "value": "OWNEREMAIL" }, "tagValue": { "value": "TEST@SAMPLE.COM" } }'

# Create the Policy Assignment
$assignment = New-AzPolicyAssignment -Name 'apply-default-tag-value' -DisplayName 'Apply tag and its default value Assignment' -Scope $scope -PolicyDefinition $definition -PolicyParameter $policyparam
```

### Remove with Powershell
```
# Remove the Policy Assignment
Remove-AzPolicyAssignment -Id $assignment.ResourceId

# Remove the Policy Definition
Remove-AzPolicyDefinition -Id $definition.ResourceId
```

## Azure CLI
### Deploy with Azure CLI
```
# Create the Policy Definition (Subscription scope)
definition=$(az policy definition create --name 'apply-default-tag-value' --display-name 'Apply tag and its default value' --description 'Applies a required tag and its default value if it is not specified by the user' --rules 'https://raw.githubusercontent.com/alchen99/az-policies/master/apply-default-tag-value/azurepolicy.rules.json' --params 'https://raw.githubusercontent.com/alchen99/az-policies/master/apply-default-tag-value/azurepolicy.parameters.json' --mode All)

# Set the scope to a resource group; may also be a resource, subscription, or management group
scope=$(az group show --name 'YourResourceGroup')

# Set the Policy Parameter (JSON format)
policyparam='{ "tagName": { "value": "OWNEREMAIL" }, "tagValue": { "value": "TEST@SAMPLE.COM" } }'

# Create the Policy Assignment
assignment=$(az policy assignment create --name 'apply-default-tag-value' --display-name 'Apply tag and its default value Assignment' --scope `echo $scope | jq '.id' -r` --policy `echo $definition | jq '.name' -r` --params "$policyparam")
```

### Remove with Azure CLI
```
# Remove the Policy Assignment
az policy assignment delete --name `echo $assignment | jq '.name' -r`

# Remove the Policy Definition
az policy definition delete --name `echo $definition | jq '.name' -r`
```

