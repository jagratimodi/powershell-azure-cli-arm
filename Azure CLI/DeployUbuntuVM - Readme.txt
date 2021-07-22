ssh-keygen -t rsa -b 2048

cat .ssh/id_rsa.pub

RGName="rg-anusha-001"
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

##### DEMO 2
##### From within the Ubuntu VM
sudo apt-get -y update
sudo apt-get -y install nginx
logout

##### View the site.
http://$publicIp"

az group create --help
az group list --output table
az vm stop -g $RGName -n $VmName
az group delete --name $RGName

SHA256:V7RfDEndusMK2eflbBRodR/MRwkdF6rO0g2k8NOglQw APPLIEDIS+Jagrati.Modi@AIS-JagratiModi


