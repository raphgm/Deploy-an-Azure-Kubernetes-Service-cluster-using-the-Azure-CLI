# Deploy-an-Azure-Kubernetes-Service-cluster-using-the-Azure-CLI
Using VScode to deploy AKS to Azure, install all the neccesary extensions including Azure account & azure cli   
## Create a resource group  
az group create --name lastclass --location eastus  
## Create AKS cluster 
az aks create --resource-group lastclass --name myAKSCluster --node-count 1 --enable-addons monitoring --generate-ssh-keys  
##  install kubectl
az aks install-cli  
## Configure kubectl  
az aks get-credentials --resource-group lastclass --name myAKSCluster  
## Verify the connection to your cluster using the kubectl get command  
kubectl get nodes
## Make sure the node status is Ready:
view the status
## configure the yaml file
code.azure-vote.yaml
## save to directory
kubectl apply -f azure-vote.yaml
## Test the application
kubectl get service azure-vote-front --watch  
The EXTERNAL-IP output for the azure-vote-front service will initially show as pending.    
Once the EXTERNAL-IP address is visible, use CTRL-C to stop the kubectl watch process.   
To see the Azure Vote app in action, open a web browser to the external IP address of your service.  


