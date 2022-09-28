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

You can have access to **subctl** CLI from Web terminal integrated in Openshift console.

Services exposed to all clusters are with the domain **<service-name>.<namespace>.svc.clusterset.local**

* List all information about submariner configuration
```
bash-4.4 ~ $ subctl show all
Cluster "https://172.50.0.1:443"
 ✓ Detecting broker(s)

 ✓ Showing Connections
GATEWAY              CLUSTER  REMOTE IP      NAT  CABLE DRIVER  SUBNETS                       STATUS     RTT avg.    
ocpr02-ocp-master-0  ocpr02   192.168.4.53   no   libreswan     172.30.0.0/16, 10.128.0.0/14  connected  410.859µs   
ocpr01-ocp-master-0  ocpr01   192.168.3.147  no   libreswan     172.40.0.0/16, 10.132.0.0/14  connected  1.193921ms  

 ✓ Showing Endpoints
CLUSTER ID                    ENDPOINT IP     PUBLIC IP       CABLE DRIVER        TYPE            
ocpr03                        192.168.4.210   5.104.101.178   libreswan           local           
ocpr02                        192.168.4.53    5.104.101.178   libreswan           remote          
ocpr01                        192.168.3.147   5.104.101.178   libreswan           remote          

 ✓ Showing Gateways
NODE                            HA STATUS       SUMMARY                         
ocpr03-ocp-master-0             active          All connections (2) are established

 ✓ Showing Network details
    Discovered network details via Submariner:
        Network plugin:  OVNKubernetes
        Service CIDRs:   [172.50.0.0/16]
        Cluster CIDRs:   [10.136.0.0/14]

 ✓ Showing versions
COMPONENT                       REPOSITORY                                            VERSION         
submariner                      registry.redhat.io/rhacm2                             v0.13.0         
submariner-operator             registry.redhat.io/rhacm2                             8b73d6fcec619b0 
service-discovery               registry.redhat.io/rhacm2                             v0.13.0
```

* Expose an existing service from a cluster to all clusters
```
bash-4.4 ~ $ subctl export service --namespace pacman-app pacman
 ✓ Service exported successfully
 ```

* Test connectivity from a remote cluster
```
bash-4.4 ~ $ curl -k pacman.pacman-app.svc.clusterset.local
<!DOCTYPE html>

<!--<html lang="en">-->
<html lang="en" manifest="cache.manifest">
<head>
    <meta charset="utf-8" />
    <title>Pac-Man</title>
    <link rel="stylesheet" href="style.css">
    <link rel="stylesheet" href="pacman-canvas.css">

    <!-- SEO Stuff -->
...
```  