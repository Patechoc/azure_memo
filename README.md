# azure_memo
Simple useful commands to run against Azure 


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
