# Create VM using Azure CLI

```
ssh-keygen -t rsa -b 2048

cat .ssh/id_rsa.pub

RGName="rg-powershell-001"
LocationName="EastUS"
BaseName="vmjun2021"
VmName="ubuntu$($BaseName)" 
username="demouser"
ImageName="UbuntuLTS" 

az group create --name $RGName --location $LocationName

az group list --output table

az vm create \
    --resource-group $RGName \
    --name $VmName \
    --image $ImageName \
    --admin-username $username \
    --authentication-type ssh --ssh-key-value ~/.ssh/id_rsa.pub

publicIp=$(az vm show \
    --resource-group $RGName \
    --name $VmName \
    --show-details \
    --query publicIps \
    --output tsv)

echo $publicIp

az vm open-port --resource-group $RGName --name $VmName --port 80

ssh -i ~/.ssh/id_rsa demouser@$publicIp

sudo apt update && sudo apt install -y lamp-server^
logout

```


## Login to vm

### From within the Ubuntu VM
sudo apt-get -y update
sudo apt-get -y install nginx
logout

### View the site.
http://$publicIp"

### Help commands
az group create --help
az group list --output table
az vm stop -g $RGName -n $VmName

### Remove resources
az group delete --name $RGName




