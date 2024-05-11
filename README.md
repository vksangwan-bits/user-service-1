## Running application in docker containers:
#### Using Docker CLI

```
docker network create --driver bridge micro_network (skip if already created)
docker build -t user-srv .
docker run -p 5001:5001 --detach --name user-service --net=micro_network user-srv
```

# Minikube hosting
```
### Start Minikube:
minikube start --driver=docker

### Set Up Docker Environment for Minikube:
minikube docker-env | Invoke-Expression
& minikube -p minikube docker-env --shell powershell | Invoke-Expression

### Build Docker Image
docker build -t user-app .

### Update Kubernetes Manifests:- set image field in frontend-deployment.yaml
image: user-app

### Tag your Docker image:- docker tag <local-image-name> <username>/<image-name>:<tag>
docker tag user-app vksan9/user-app:latest

### Log in to Docker Hub (or the registry):
docker login

### Push the Docker:- image:docker push <username>/<image-name>:<tag>
docker push vksan9/user-app:latest

### Apply Kubernetes Manifests:- 
kubectl apply -f user-deployment.yaml
kubectl apply -f user-service.yaml

### Access Your Application:- Access your application using the IP address and NodePort
### specified in the service manifest e.g http://<minikube-ip>:30000
minikube ip
minikube service user-app --url


### Check Pod Status
kubectl get pods

