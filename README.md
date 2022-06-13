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
apiVersion: apps/v1
kind: Deployment
metadata:
  name: azure-vote-back
spec:
  replicas: 1
  selector:
    matchLabels:
      app: azure-vote-back
  template:
    metadata:
      labels:
        app: azure-vote-back
    spec:
      nodeSelector:
        "kubernetes.io/os": linux
      containers:
      - name: azure-vote-back
        image: mcr.microsoft.com/oss/bitnami/redis:6.0.8
        env:
        - name: ALLOW_EMPTY_PASSWORD
          value: "yes"
        resources:
          requests:
            cpu: 100m
            memory: 128Mi
          limits:
            cpu: 250m
            memory: 256Mi
        ports:
        - containerPort: 6379
          name: redis
---
apiVersion: v1
kind: Service
metadata:
  name: azure-vote-back
spec:
  ports:
  - port: 6379
  selector:
    app: azure-vote-back
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: azure-vote-front
spec:
  replicas: 1
  selector:
    matchLabels:
      app: azure-vote-front
  template:
    metadata:
      labels:
        app: azure-vote-front
    spec:
      nodeSelector:
        "kubernetes.io/os": linux
      containers:
      - name: azure-vote-front
        image: mcr.microsoft.com/azuredocs/azure-vote-front:v1
        resources:
          requests:
            cpu: 100m
            memory: 128Mi
          limits:
            cpu: 250m
            memory: 256Mi
        ports:
        - containerPort: 80
        env:
        - name: REDIS
          value: "azure-vote-back"
---
apiVersion: v1
kind: Service
metadata:
  name: azure-vote-front
spec:
  type: LoadBalancer
  ports:
  - port: 80
  selector:
    app: azure-vote-front

## save to directory
kubectl apply -f azure-vote.yaml
## Test the application
kubectl get service azure-vote-front --watch
The EXTERNAL-IP output for the azure-vote-front service will initially show as pending.  
Once the EXTERNAL-IP address changes from pending to an actual public IP address, use CTRL-C to stop the kubectl watch process. The following example output shows a valid public IP address assigned to the service  
To see the Azure Vote app in action, open a web browser to the external IP address of your service.


