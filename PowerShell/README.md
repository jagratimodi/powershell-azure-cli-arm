# PowerShell commands to create Azure VM-to connect to Azure

```
Connect-AzAccount
```

## Declaring variables for creating RG and VM
```
$SubscriptionName = "Free Trial"
$RGName = "rg-anusha-003"
$LocationName = "EastUS"
$BaseName = "18jun2021win"
$VmName = "vm$($BaseName)"
$VNetName = "vnet$($BaseName)"
$SubNetName = "default"
$NsgName = "nsg$($BaseName)"
$PublicDns = "publicdns$($BaseName)$(Get-Random)"
$PortsToOpen = 80, 3389
$username = 'demouser'
$password = ConvertTo-SecureString 'Password@123' -AsPlainText -Force
$ImageName = "Win2019Datacenter"
```

##  Create new resource group
```
New-AzResourceGroup -Name $RGName -Location $LocationName -Tag @{environment="dev"; Contact="Anusha"}
```
 
## Passing credentials
```
$CredentialsForVm = New-Object System.Management.Automation.PSCredential ($username, $password)
```
 
## Create new VM
```
New-AzVm -ResourceGroupName $RGName -Name $VmName -Location $LocationName `
-Credential $CredentialsForVm -Image $ImageName `
-VirtualNetworkName $VNetName -SubnetName $SubNetName -SecurityGroupName $NsgName `
-PublicIpAddressName $PublicDns -OpenPorts $PortsToOpen
```
 
## To retrieve ip address of our VM
```
Get-AzPublicIpAddress `
-ResourceGroupName $RGName `
-Name $PublicDns | Select-Object IpAddress
```
 
## To connect to VM
```
mstsc /v [:publicIpAddress]
```
 
## To install IIS inside VM, run below command in windows powershell inside VM
```
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```
