# Azure Umanis Managed Identity

[![Build Status](https://dev.azure.com/umanis-consulting/terraform/_apis/build/status/mod_azu_private_endpoint?repoName=mod_azu_private_endpoint&branchName=master)](https://dev.azure.com/umanis-consulting/terraform/_build/latest?definitionId=13&repoName=mod_azu_private_endpoint&branchName=master)[![Unilicence](https://img.shields.io/badge/licence-The%20Unilicence-green)](LICENCE)


Common Azure terraform module to create a Private Endpoint

## Naming
Resource naming is based on the Microsoft CAF naming convention best practices. Custom naming is available by setting the parameter `custom_name`. We rely on the official Terraform Azure CAF naming provider to generate resource names when available.

## Usage
```hcl

module "umanis_tagging" {
  source = "Umanis/tags/azurerm"

  location          = "France Central"
  client            = "XY2"
  project           = "1234"
  budget            = "FE4567"
  rgpd_personal     = true
  rgpd_confidential = false
}

module "umanis_naming" {
  source = "Umanis/naming/azurerm"

  location    = "France Central"
  client      = "XY2"
  project     = "1234"
  area        = 1
  environment = "tst"
}

module "umanis_resource_group" {
  source = "Umanis/resource-group/azurerm"

  tags         = module.umanis_tagging.tags
  location     = "France Central"
  description  = "Test resource group"
  caf_prefixes = module.umanis_naming.resource_group_prefixes
}

module "umanis_private_endpoint" {
  source = "Umanis/private-endpoint/azurerm"

  resource_group_name = module.umanis_resource_group.name
  description         = "Test Private Endpoint"
  caf_prefixes        = module.umanis_naming.resource_prefixes
  resource_id         = << azure storage account id >>
  subnet_id           = << subnet resource id >>
  subresource_names   = ["dfs"]
}

```
<!-- BEGIN_TF_DOCS -->
## Requirements

| Name | Version |
|------|---------|
| <a name="requirement_terraform"></a> [terraform](#requirement\_terraform) | >= 1.0.0 |
| <a name="requirement_azurecaf"></a> [azurecaf](#requirement\_azurecaf) | >= 1.2.10 |
| <a name="requirement_azurerm"></a> [azurerm](#requirement\_azurerm) | >=2.90.0 |

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| <a name="input_resource_group_name"></a> [resource\_group\_name](#input\_resource\_group\_name) | Specifies the Name of the Resource Group within which the Private Endpoint should exist. Changing this forces a new resource to be created. | `string` | n/a | yes |
| <a name="input_resource_id"></a> [resource\_id](#input\_resource\_id) | The ID of the Private Link Enabled Remote Resource which this Private Endpoint should be connected to. | `string` | n/a | yes |
| <a name="input_subnet_id"></a> [subnet\_id](#input\_subnet\_id) | The ID of the Subnet from which Private IP Addresses will be allocated for this Private Endpoint. Changing this forces a new resource to be created. | `string` | n/a | yes |
| <a name="input_subresource_names"></a> [subresource\_names](#input\_subresource\_names) | A list of subresource names which the Private Endpoint is able to connect to. subresource\_names corresponds to group\_id. Changing this forces a new resource to be created. | `list(string)` | n/a | yes |
| <a name="input_caf_prefixes"></a> [caf\_prefixes](#input\_caf\_prefixes) | Prefixes to use for caf naming. | `list(string)` | `[]` | no |
| <a name="input_custom_location"></a> [custom\_location](#input\_custom\_location) | Specifies a custom location for the resource. | `string` | `""` | no |
| <a name="input_custom_name"></a> [custom\_name](#input\_custom\_name) | Specifies a custom name for the resource. | `string` | `""` | no |
| <a name="input_custom_tags"></a> [custom\_tags](#input\_custom\_tags) | The custom tags to add on the resource. | `map(string)` | `{}` | no |
| <a name="input_description"></a> [description](#input\_description) | Resource description. | `string` | `""` | no |

## Outputs

| Name | Description |
|------|-------------|
| <a name="output_linked_resource_name"></a> [linked\_resource\_name](#output\_linked\_resource\_name) | Azure name of the private resource. |
| <a name="output_private_ip"></a> [private\_ip](#output\_private\_ip) | Endpoint private IP in the subnet. |
<!-- END_TF_DOCS -->

## Related documentation

Terraform Azure public IP documentation: [https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/private_endpoint](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/private_endpoint)

Terraform Azure CAF Naming documentation: [https://registry.terraform.io/providers/aztfmod/azurecaf/latest/docs/resources/azurecaf_name](https://registry.terraform.io/providers/aztfmod/azurecaf/latest/docs/resources/azurecaf_name)

