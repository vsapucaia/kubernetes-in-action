## Chapter04

- Replication and other controllers: deploying managed pods

```sh
# create unhealthy kubia pod 
k create -f kubia-liveness-probe.yaml

# check previous pod log after a restart
k logs kubia-liveness --previous

# if a pod terminates with 137 or 143, it was killed externally (by k9s), not a process error
# most likely due to a liveness probe

# create a ReplicationController
k create -f kubia-rc.yaml
# delete the REplicationController
k delete rc kubia
# edit the ReplicationController
k edit rc kubia
# scale the ReplicationController to 10
k scale rc kubia --replicas=10
# delete rc with cascade false
k delete rc kubia --cascade=orphan

# create a ReplicaSet
k create -f kubia-replicaset.yaml

# create a DaemonSet
k create -f ssd-monitor-daemonset.yaml
k label node docker-desktop disk=ssd

# create a Job
k create -f batch-job.yaml
# get all pods (including completed ones)
k get pods --show-all
```
