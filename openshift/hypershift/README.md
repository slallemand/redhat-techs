# Hypershift

## Doc

- https://hypershift-docs.netlify.app/how-to/kubevirt/create-kubevirt-cluster/


## Prerequisits

* HUB Cluster with ACM
* install agent with discovery image

## Deploy Hypershift add-on with ACM

1. Enable hypershift add-on

```
oc patch mce multiclusterengine --type=merge -p '{"spec":{"overrides":{"components":[{"name":"hypershift-preview","enabled": true}]}}}'
```

2. Install Hypershift add-on where local-cluster is your ACM cluster

```
apiVersion: addon.open-cluster-management.io/v1alpha1
kind: ManagedClusterAddOn
metadata:
  name: hypershift-addon
  namespace: local-cluster
spec:
  installNamespace: open-cluster-management-agent-addon
```

3. Check if hypershift add-on is running

```
oc get managedclusteraddons -n local-cluster hypershift-addon
```

## Create Hosted cluster with Hypershift in ACM

### 1. Create VM manually with agent discovery image and boot on it

![create_vm_1](images/hypershift-1.png)  

![create_vm_2](images/hypershift-2.png) 

![create_vm_3](images/hypershift-3.png) 


### 2. Created VM are automatically integrated in ACM's hosts inventory via agent discovery image

![discovery_1](images/hypershift-4.png)

### 3. Aprove discovered host

![discovery_2](images/hypershift-5.png)

![discovery_3](images/hypershift-6.png)

### 4. Update VMs to eject CDROM to boot on disk next reboot

![update_vm_1](images/hypershift-17.png)

### 5. Create Hosted cluster via ACM WebUI

![create_cluster_1](images/hypershift-7.png)

![create_cluster_2](images/hypershift-8.png)

![create_cluster_3](images/hypershift-9.png)

![create_cluster_4](images/hypershift-10.png)

![create_cluster_5](images/hypershift-11.png)

![create_cluster_6](images/hypershift-12.png)

![create_cluster_7](images/hypershift-13.png)

![create_cluster_8](images/hypershift-14.png)

### 6. Check Hosted cluster deployment 

![create_cluster_9](images/hypershift-15.png)

![create_cluster_10](images/hypershift-16.png)