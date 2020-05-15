# starburst-aks-demo

Setup a demo environment for Starburst (Presto) on Azure Kubernetes Service.  

## Setup Starburst in Your Subscription

Login to the Azure Portal and open a `bash` CloudShell.  Run the following script, changing the variables as needed at the top:

```bash

# vars to change
SUBSCRIPTION="davew demo"
AKS_NAME="starburst" 
RES_GROUP_NAME="starburstdemo"
LOCATION="eastus"
VNET_NAME="aksvnet"
SUBNET_NAME="starburst"

SUBNET_RES_ID=$(az network vnet subnet list \
  --vnet-name $VNET_NAME \
  --resource-group $RES_GROUP_NAME \
  --query "([? name == '$SUBNET_NAME' ])[0].id" | cut -d '"' -f 2)

az aks create --resource-group $RES_GROUP_NAME \
  --name $AKS_NAME  \
  --enable-cluster-autoscaler \
  --node-count 5 \
  --min-count 5 \
  --max-count 8 \
  --node-vm-size Standard_DS3_v2 \ 
  --generate-ssh-keys \
  --vnet-subnet-id $SUBNET_RES_ID \
  --network-plugin azure \
  --enable-addons monitoring
  
```

To destroy the demo environment, just delete the resource group.  





az aks get-credentials --resource-group ${RES_GROUP_NAME} --name ${AKS_NAME}
mv signed_trial.license signed.license


2.     Add Starburst license:

##kubectl create secret generic presto-license --from-file signed.license

3.     Apply files:

kubectl apply -f https://starburstdata.s3.us-east-2.amazonaws.com/mission-control/0.18/k8s/postgres.yaml

kubectl apply -f https://starburstdata.s3.us-east-2.amazonaws.com/mission-control/0.18/k8s/missioncontrol.yaml

kubectl get pods

kubectl get svc

now we do mission control setup
    adls to write
    blob to read


admin/admin

Password01!!

kubectl get pods
demo for later:  HPA with vmscalesets 

when cluster is up then 
kubectl get svc to get lb ip for presto cluster and not mission control

http://52.184.201.4:8080/ui/

dbeaver or jupyter 
need jdbc driver


power bi

select * from tpch.sf1.customer limit 10

drop schema hive.adlsgen2


create schema hive.adlsgen2  WITH (location = 'abfs://tpch-10gb@davewdemodata.dfs.core.windows.net/presto');


create table hive.adlsgen2.customer as select * from tpch.sf1.customer 

sql server query federation example

create table hive.adlsgen2.customer as select * from tpch.sf1.customer 

select * from airline_oltp.dbo.airline limit 100

create table hive.adlsgen2.airline as select * from airline_oltp.dbo.airline limit 10

tnatssb
Manish-starburst


```