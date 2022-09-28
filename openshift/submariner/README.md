# Notes about submariner

## Install

You can install submariner directly from RHACM and clusterSet Add-on. As prerequisite, you have to create this manifest on HUB cluster for each managed cluster.
```
apiVersion: submarineraddon.open-cluster-management.io/v1alpha1
kind: SubmarinerConfig
metadata:
    name: submariner
    namespace: <managed-cluster-namespace>
spec: {}
```

## Subctl CLI

