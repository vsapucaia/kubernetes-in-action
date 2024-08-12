## Chapter05

- Services: enabling clients to discover and talk to pods

```sh
# create a service
k apply -f service.yaml
k create -f ../Chapter04/kubia-replicaset.yaml

# exec from inside of a pod
# get a random pod and the IP address of the service
k exec kubia-6dpvn -- curl -s http://10.107.197.33

# check environment variables inside of a pod
k exec kubia-6dpvn -- env
k exec kubia-6dpvn -- env | grep KUBIA

# shell in the pod and check the DNS resolution
k exec -it kubia-6dpvn -- sh
curl http://kubia.default.svc.cluster.local
curl http://kubia.default
curl http://kubia

# ping wont work to check if a service is up

# create a NodePort service
k create -f kubia-svc-nodeport.yaml
k get svc kubia-nodeport
k get nodes -o wide

# get node IP and NodePort
NODE_IP=$(k get nodes -o jsonpath='{.items[0].status.addresses[?(@.type=="InternalIP")].address}')

# this would work in a GKE cluster node
NODE_EXTERNAL_IP=$(k get nodes -o jsonpath='{.items[*].status.addresses[?(@.type=="ExternalIP")].address}')
# but nodes on docker-desktop run on the same host, so we need to use the host IP
curl localhost:30123
```