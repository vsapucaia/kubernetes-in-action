## Chapter02

- Learning how to initiate a basic docker container with node.js

- notes: build process isn't performed by the Docker client...

- need to understand pod vs replicationcontroller (why I didn't create a rc?)

- need to understand replicationcontroller vs deployment

### Creating, running, and sharing a container image
```sh
docker build -t kubia Chapter02/kubia
docker images
docker run --name kubia-container -p 8080:8080 -d kubia
docker ps
# get low-level information about the container
docker inspect kubia-container
# shell into the container
docker exec -it kubia-container bash

# tagging the image before pushing it to docker.hub
docker tag kubia vsfortunato/kubia
docker push vsfortunato/kubia
# you can now run your image from anywhere with:
docker run -p 8080:8080 -d vsfortunato/kubia
```

### Installing kubernetes client / Minikube

I've ran into issues when trying to install with docker as the driver, so I had to go with Qemu
https://devopscube.com/minikube-mac/

```sh
brew install qemu
brew install socket_vmnet
brew tap homebrew/services
HOMEBREW=$(which brew) && sudo ${HOMEBREW} services start socket_vmnet

brew install minikube
minikube start --driver qemu --network socket_vmnet

# switching contexts
kubectl config get-contexts
kubectl config use-context minikube
kubectl cluster-info
# change back to gcloud
kubectl config use-context gke_taxfix-development_europe-west3_gke-dev-cluster-1

# ssh into minikube
minikube ssh
ps aux
```

### creating a gcloud kubernetes cluster

```sh
gcloud container clusters list
gcloud container clusters create kubia --num-nodes 3

kubectl get nodes
gcloud compute ssh gke-kubia-default-pool-ffce7c4a-622w

# this creates a pod, not a rc
# k run kubia --image=vsfortunato/kubia --port=8080
# this creates the rc
k create -f Chapter02/kubia.yaml
k expose pod kubia --type=LoadBalancer --name kubia-http
```

### horizontally scaling

```sh
k scale rc kubia --replicas=3
```