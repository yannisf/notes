# Azure CLI

Azure CLI is a command line to control remotely Azure resources

## Install

Installation instructions are available [here](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli-windows?view=azure-cli-latest).

**NOTE**: When installing in Linux Mint Tara edit the apt source to point to *bionic* instead of *tara*.

## Useful commands

### Login

    $ az login

### VM

    $ az vm {start|stop|restart} --name ${vm_name} --resource-group ${resource_group}

Further information can be found [here](https://docs.microsoft.com/en-us/cli/azure/vm?view=azure-cli-latest).

### Network

    $ az network public-ip show --name ${ip_name} --resource-group ${resource_group}
    $ az network public-ip update --name ${ip_name} --resource-group ${resource_group} --reverse-fqdn ${my_domain.}
