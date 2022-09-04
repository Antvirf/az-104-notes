# [Automate Azure tasks using scripts with PowerShell](https://docs.microsoft.com/en-us/learn/modules/automate-azure-tasks-with-powershell/)

## Learning objectives

* Decide if Azure PowerShell is the right tool for your Azure administration tasks
* Install Azure PowerShell on Linux, macOS, and/or Windows
* Connect to an Azure subscription using Azure PowerShell
* Create Azure resources using Azure PowerShell

## [Choosing between Azure Portal, CLI and PowerShell](https://docs.microsoft.com/en-us/learn/modules/automate-azure-tasks-with-powershell/2-decide-if-azure-powershell-is-right-for-your-tasks)

Almost complete feature parity so technically you can do anything with any one of them. Microsoft wants you to answer either CLI or PowerShell for anything that would require repetition (though in reality you should probably do that with IaC). For automated tasks, team skill set is the primary deciding factor.

## Installing Azure PowerShell

* Windows: [link](https://docs.microsoft.com/en-us/learn/modules/automate-azure-tasks-with-powershell/4-exercise-install-azure-powershell?pivots=windows)
* Linux: ```sudo apt-get install powershell```, becomes ```pwsh```
* MacOs: ```brew install --cask powershell```

Once installed, you need to install the Azure Module. The Azure PowerShell module consists of lots of different cmdlets. To install it;

```bash

# launch powershell
$ pwsh
```

```PowerShell
# install the Az module
> Install-Module -Name Az -Scope CurrentUser -Repository PSGallery -Force 

# update the Az module
> Update-Module -Name Az
```

## Azure Powershell usage

### Syntax pointers

Has an 'interactive' mode to use as a regular CLI, and also supports scripts. Commands are called **cmdlets** (command-lets) for some reason.

cmdlets follow a verb-noun convention: e.g. ```Get-Process```, ```Format-Table```, ```Start-Service```.

* ```New-AzVm``` - data creation cmdlets always start with ```New```
* ```Set-AzVm``` - data update cmdlets always start with ```Set```
* ```Update-AzVm``` - when Azure feels like it, the cmdlet is ```Update``` instead
* ```Get-AzVm``` - retrieving cmdlets always starts with ```Get```

Other sample commands:

* ```Get-AzResourceGroup | Format-Table``` - get resource groups and format as a table
* ```New-AzResourceGroup -Name <name> -Location <location>``` - create new RG

### Authentication and context setup

To start using the module, you need to authenticate. You can do this with ```Connect-AzAccount```.

Next, set subscription context;

```PowerShell
Set-AzContext -Subscription '00000000-0000-0000-0000-000000000000'
```

### Example: Get VM info, change details and push update

First, get the VM info saved into an object;

```PowerShell
$vm = Get-AzVM  -Name MyVM -ResourceGroupName ExerciseResources
```

Then, change its size SKU property;

```PowerShell
$vm.HardwareProfile.vmSize = "Standard_DS3_v2"
```

Finally, update the size with ```Update-AzVM```

```PowerShell
Update-AzVM -ResourceGroupName $ResourceGroupName  -VM $vm
```

### Example: Create and delete a VM

```PowerShell
# connect to account, if not yet done
> Connect-AzAccount

# create vm
> New-AzVm `
    -ResourceGroupName [sandbox resource group name] `
    -Name "test-virtual-machine-eus-01" `
    -Credential (Get-Credential) `
    -Location "East US" `
    -Image UbuntuLTS `
    -OpenPorts 22 `
    -PublicIpAddressName "test-virtual-machine-01"

# save vm details into object
> $vm = (Get-AzVM -Name "test-virtual-machine-eus-01" -ResourceGroupName [sandbox resource group name])

# explore the VM object
> $vm
# ResourceGroupName : [sandbox resource group name]
# Id                : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/[sandbox resource group name]/providers/Microsoft.Compute/virtualMachines/test-virtual-machine-eus-01
# VmId              : 00000000-0000-0000-0000-000000000000
# Name              : test-virtual-machine-eus-01
# Type              : Microsoft.Compute/virtualMachines
# Location          : eastus
# Tags              : {}
# HardwareProfile   : {VmSize}
# NetworkProfile    : {NetworkInterfaces}
# OSProfile         : {ComputerName, AdminUsername, LinuxConfiguration, Secrets}
# ProvisioningState : Succeeded
# StorageProfile    : {ImageReference, OsDisk, DataDisks}

# access specific properties with the dot notation
> $vm.HardwareProfile
> $vm.StorageProfile.OsDisk

# use the vm object to pipe into other commands - e.g. get its size:
> $vm | Get-AzVMSize

# get public ip
> Get-AzPublicIpAddress `
    -ResourceGroupName [sandbox resource group name] `
    -Name "test-virtual-machine-01"

```

## Azure PowerShell Script samples

Providing input variables:

```PowerShell
.\setupEnvironment.ps1 -size 5 -location "East US"
```

Accessing them inside the script:

```PowerShell
param([string]$location, [int]$size)
```

Longer example with different actions in PowerShell script

```PowerShell
# get variables from input
param([string]$location, [int]$size)

# define variables - commented out as we get them from input
# $loc = "East US"
# $size = 3
$adminCredential = Get-Credential # define variable via a cmdlet

# execute cmdlets with variables
New-AzResourceGroup `
    -Name "MyResourceGroup" `
    -Location $loc

# loops
For ($i = 1; $i -lt 3; $i++)
{
    $i
}

```

Second pre-made example - creates 3 VMs inside the specified resource groups.

```PowerShell
param([string]$resourceGroup)

$adminCredential = Get-Credential -Message "Enter a username and password for the VM administrator."

For ($i = 1; $i -le 3; $i++)
{
    $vmName = "ConferenceDemo" + $i
    Write-Host "Creating VM: " $vmName
    New-AzVm -ResourceGroupName $resourceGroup -Name $vmName -Credential $adminCredential -Image UbuntuLTS
}
```

The above would be executed with:

```PowerShell
./ConferenceDailyReset.ps1 [sandbox resource group name]
```
