# starburst-aks-demo

Setup a demo environment for Starburst (Presto) on Azure Kubernetes Service.  

## Setup Starburst in Your Subscription

Login to the Azure Portal and open a `bash` CloudShell.  Run the following script, changing the variables as needed at the top:

```bash

# vars to change
SUBSCRIPTION="davew demo"
AKS_NAME="starburst-demo" 
RES_GROUP_NAME="starburstdemo"
LOCATION="eastus2"
VNET_NAME="aksvnet"
SUBNET_NAME="starburst"

az account set --subscription "$SUBSCRIPTION"

# TODO vnet command, take defaults
az network vnet create \
  --name myVirtualNetwork \
  --resource-group myResourceGroup \
  --subnet-name default

# TODO subnet command, take defaults

SUBNET_RES_ID=$(az network vnet subnet list \
  --vnet-name $VNET_NAME \
  --resource-group $RES_GROUP_NAME \
  --query "([? name == '$SUBNET_NAME' ])[0].id" | cut -d '"' -f 2)

az aks create --resource-group $RES_GROUP_NAME \
  --name $AKS_NAME  \
  --enable-cluster-autoscaler \
  --location $LOCATION \
  --node-count 5 \
  --min-count 5 \
  --max-count 8 \
  --node-vm-size Standard_DS3_v2 \
  --generate-ssh-keys \
  --vnet-subnet-id $SUBNET_RES_ID \
  --network-plugin azure \
  --enable-addons monitoring

az aks get-credentials --resource-group ${RES_GROUP_NAME} --name ${AKS_NAME}

#to check status
kubectl get nodes 

kubectl apply -f https://starburstdata.s3.us-east-2.amazonaws.com/mission-control/0.20/k8s/postgres.yaml

kubectl apply -f https://starburstdata.s3.us-east-2.amazonaws.com/mission-control/0.20/k8s/missioncontrol.yaml

kubectl get pods

# need EXTERNAL-IP for LoadBalancer
kubectl get svc

then go to ip:5042


admin/admin

Password01!!

```





now we do mission control setup
  Data sources
    adls to write
    call it lake
      davewdemodata

create a cluster to test


    internal metastore, ephemeral
    blob to read
davewdemoblobs 




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

create schema hive.adlsgen2  WITH (location = 'abfs://starburst@davewdemodata.dfs.core.windows.net/');
create table hive.adlsgen2.customer as select * from tpch.sf1.customer;
create table hive.adlsgen2.lineitem as select * from tpch.sf1.lineitem;


select * from hive.adlsgen2.customer;

create table hive.adlsgen2.lineitem as select * from tpch.sf1.lineitem;
create table hive.adlsgen2.nation as select * from tpch.sf1.nation;
create table hive.adlsgen2.orders as select * from tpch.sf1.orders;
create table hive.adlsgen2.part as select * from tpch.sf1.part;
create table hive.adlsgen2.partsupp as select * from tpch.sf1.partsupp;
create table hive.adlsgen2.region as select * from tpch.sf1.region;
create table hive.adlsgen2.supplier as select * from tpch.sf1.supplier;


sql server query federation example

create table hive.adlsgen2.customer as select * from tpch.sf1.customer 

select * from airline_oltp.dbo.airline limit 100

create table hive.adlsgen2.airline as select * from airline_oltp.dbo.airline limit 10

tnatssb
Manish-starburst

To destroy the demo environment, just delete the resource group.  
```