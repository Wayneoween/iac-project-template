# Terraform/OpenTofu Template

This is a template containing a basic skeleton for Terraform/OpenTofu wrapped in Terragrunt.

## General structure

### Environments

Modules that are holding state are stored in one of the environments. The environments `global`, `staging` and `production` are a good starting point.

## Create own Modules

See `environment/global/state-storage` for an example of an module.

## Hierarchical loading of data

- module loads `inputs.yaml`
- environment loads `environment_inputs.yaml`
- root loads `global_inputs.yaml`

All the loaded data is merged and passed to the called modules. The YAML files are merged with priority to specific data. That means if a value is set on root level and on module level, the module level wins.

Note that deep merging of nested values does not work. If *any* sub-values are set in a higher priority file, the complete value is overwritten and *all* sub-values that are not defined in the higher priority file will not be present after merging.

## Root Variables

The following variables are generated by terragrunt and added to every module:

- `environment`
- `project`
- `module`
- `region`

## Generated Code

Generated files are named "*_generated.tf", which is already excluded from git.

The templates for generated code live in `code_snippets` either included with `file()` or `templatefile()`.

## Secrets

Secrets can be stored in `secrets.yaml`. Its content is added to the inputs hash.