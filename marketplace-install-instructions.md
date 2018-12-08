# Azure Marketplace install of PCF
*Compiled by Kendall Adkins on December 7th, 2018*
## Find the Marketplace Install
1. Open the [Azure Portal](https://portal.azure.com).
1. Click on "Create a resource" in the upper left hand corner of the portal page.
1. In the search bar type "pivotal cloud foundry" and hit enter.
1. Click on "Pivotal Cloud Foundry on Microsoft Azure".
1. You are now ready to begin preparing for the installation.
## Marketplace Install Instructions
### Prerequisites
Read through the install instructions provided on Azure for the marketplace install of PCF. You should specifically follow
the instructions provided in the "Prerequisites" section. The following subsections of this document quickly cover the critical parts of the procedures.
#### Install the Azure CLI

[Install Azure CLI on Windows](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli-windows?view=azure-cli-latest)

#### Install the Azure Powershell module on Windows

1. Open power shell with "run as Administrator"
1. Run "Install-Module -Name AzureRM -AllowClobber -Force"
1. Enter "Y"

#### Install Azure-Sp-Tool
[Install azure-sp-tool](https://github.com/danhigham/azure-sp-tool)

#### Run the Azure-Sp-Tool
Run the following commands:

`az login`

`az account list`

Make sure your Azure PA subscription (account) is set as the default.

Example:
```
{
    ...     
    "isDefault": true,
    "name": "PA-dhowser",
    ...
}
```

If it is not, then use the following command to set the default:

`az account set --subscription <enter your PA subscription id>`

Create the service principal with the following command:

`azure-sp-tool create-sp`

The tool will output a file called "azure-credentials.json" to be used when executing the Azure Marketplace install.

### Generate a Public/Private Key Pair for SSH

[How to use SSH keys with Windows on Azure ](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/ssh-from-windows)

### Get a Pivotal Network Token (UAA API Token)

1. Goto [Pivotal Network](https://network.pivotal.io/).
1. Login.
1. Select "Edit Profile" in the drop down menu in the upper right hand corner by your username.
1. Scroll to the botton of the page and click on UAA API TOKEN -> "Request New Refresh Token".
1. Save the token in a safe place.

### Execute the Marketplace Install

1. Click on "Create".

1. Enter the following into the form:

    SSH admin Username = ubuntu  
    SSH public key = &lt;*Your generated public key*&gt;  
    Service Principal = &lt;*The file you generated with the azure-sp-tool*&gt;  
    Resource Group = &lt;*Click "Create new" and create a unique resource group for the install*&gt;  
    Location = &lt;*Select a location close to you*&gt;

1. Click "OK" and validation will be run on the entered form paramaters.

1. Click "OK" and you will be presented with the T&C's.

1. Click "Create" to run the installation.

1. Then wait a very long time...

### Post Installation

1. Open the [Azure Portal](https://portal.azure.com).

1. Click on "Resource Groups".

1. Click on the link for the new resource group created for the install.

1. Under "Deployments" you should see a link with the number of deployments executed (e.g. "<u>2 Succeeded</u>"). Click on the link.

1. There will be a list of deployments. Click on the earliest deployment. It should be prefixed with "pivotal.pivotal-coud-foundry".

1. In the left toolbar you will see a list of four menu options. Click on the "Outputs" menu option. This will show you information you need to access the operations manager.
