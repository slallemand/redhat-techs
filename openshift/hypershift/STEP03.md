# Enable HA for deployed hosted cluster

When you deploy Hosted OpenShift cluster with ACM, control plane is deployed with no HA (1 pod for ETC and OVN for example)

![ha_cluster_1](images/hypershift-21.png)

To have High Availability on the control plane, we have to modify **HostedCluster** object to replace **SingleReplica** by **HighlyAvailable**

```
apiVersion: hypershift.openshift.io/v1alpha1
kind: HostedCluster
metadata:
  name: ocp04
  namespace: ocp04
spec:
  fips: false
  release:
    image: 'quay.io/openshift-release-dev/ocp-release:4.11.25-x86_64'
  dns:
    baseDomain: rhntnx.hpecic.net
  controllerAvailabilityPolicy: **SingleReplica**
  infraID: ocp04
  etcd:
    managed:
      storage:
        persistentVolume:
          size: 4Gi
        type: PersistentVolume
    managementType: Managed
  infrastructureAvailabilityPolicy: **SingleReplica**
  platform:
    agent:
      agentNamespace: hpe-cic-nutanix
    type: Agent
    ...
```

```
apiVersion: hypershift.openshift.io/v1alpha1
kind: HostedCluster
metadata:
  name: ocp04
  namespace: ocp04
spec:
  fips: false
  release:
    image: 'quay.io/openshift-release-dev/ocp-release:4.11.25-x86_64'
  dns:
    baseDomain: rhntnx.hpecic.net
  controllerAvailabilityPolicy: **HighlyAvailable**
  infraID: ocp04
  etcd:
    managed:
      storage:
        persistentVolume:
          size: 4Gi
        type: PersistentVolume
    managementType: Managed
  infrastructureAvailabilityPolicy: **HighlyAvailable**
  platform:
    agent:
      agentNamespace: hpe-cic-nutanix
    type: Agent
    ...
```

We have now 3 replicats of critical parts to provide High Availability on hosted control plane

![ha_cluster_2](images/hypershift-22.png)


## Network part

**ocp04** Hosted cluster deployed via ACM have this network configuration

| What | Where hosted | accessible via |
| ---- | ------------ | --------------- |
| **ocp04** control plane | **ocp04-ocp04** namespace in ACM cluster where control plane pods are created | node port & routes on ACM cluster |
| **ocp04** data plane | on VMs created with agent and discovery image | routes |

![network_1](images/hypershift-24.png)

### HA Proxy config

HA Proxy config for LB where Control plane API backend corresponding to **node port** created on ACM clusternodes. Ingress backend corresponding to VMs and who host ingress component. 

```
...
#---------------------------------------------------------------------
# main frontend which proxys to the backends
#---------------------------------------------------------------------

frontend api
    bind 10.6.112.10:6443
    default_backend controlplaneapi

frontend secure
    bind 10.6.112.10:443
    default_backend secure

frontend insecure
    bind 10.6.112.10:80
    default_backend insecure

#---------------------------------------------------------------------
# static backend
#---------------------------------------------------------------------

backend controlplaneapi
    balance source
    server master0 10.6.115.68:30627 check
    server master1 10.6.115.171:30627 check
    server master2 10.6.115.20:30627 check

backend secure
    balance source
    server worker0 10.6.115.145:443 check
    server worker1 10.6.115.161:443 check
    server worker2 10.6.115.57:443 check

backend insecure
    balance source
    server worker0 10.6.115.145:80 check
    server worker1 10.6.115.161:80 check
    server worker2 10.6.115.57:80 check
```

## Extend cluster with new node

You can extend the hosted cluset's capacities by adding new discovered node (VM) with agent discovvery image.

![extend_cluster_1](images/hypershift-25.png)

![extend_cluster_2](images/hypershift-26.png)

![extend_cluster_3](images/hypershift-27.png)

![extend_cluster_4](images/hypershift-28.png)