## Chapter03

- Understanding pods, containers and the importance of separating process into multiple pods

### Creating, running, and sharing a container image
```sh
# Docs helper to understand the attributes of a specific resource
k explain pod
k explain pod.spec

# creating a pod from a YAML file
k create -f Chapter03/kubia-manual.yaml
# full YAML of the pod
k get po kubia-manual -o yaml
# check pod logs
k logs kubia-manual
k logs kubia-manual -c kubia

# port-forwarding
k port-forward kubia-manual 8888:8080
curl localhost:8888

# create pod with labels
k create -f kubia-manual-with-labels.yaml
k get po --show-labels

# modifying labels on exiting pods
k label po kubia-manual creation_method=manual

# filter pods by labels in k9s
/-l app=kubia

# labeling nodes and attaching pods to labeled nodes
k label node gke-kubia-85f6-node-0rrx gpu=true
k get nodes -l gpu=true
k create -f kubia-gpu.yaml

# create a custom namespace
k create ns custom-namespace
```