# Hypershift

## Doc

- https://hypershift-docs.netlify.app/how-to/kubevirt/create-kubevirt-cluster/


## Prerequisits

* HUB Cluster with ACM
* install agent with discovery image

## Deploy cluster with Hypershift

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