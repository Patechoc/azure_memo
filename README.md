# azure_memo
Simple useful commands to run against Azure 

- [Reference list of Azure services](https://docs.microsoft.com/en-us/cli/azure/azure-services-the-azure-cli-can-manage)

## Installation tricks

- instruct `pip` to use the system certificate store instead of the "incomplete" bundled cacert that comes with it by installing `pip-system-certs`

`pip install pip-system-certs`

## Login to Azure & switch subscription

### Log in

`az login`

### List your subscriptions 

`az account list --output table`

### Switch subscription

`az account set --subscription <name or id>`

## Management and Governance

### (Resource) Groups

#### List resource groups

`az group list -o table`

## Compute

### Function Apps

- List all FunctionApps: `az functionapp list -o table`
