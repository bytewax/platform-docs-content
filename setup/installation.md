How to prepare your environment before installing Bytewax Platform

# Prerequisites

The Bytewax Platform works in an ecosystem made up of the following components: 

- Identity Provider implementing OpenID Connect flow
- External access network configured in your Kubernetes cluster (Ingress Controller, DNS server)
- Default Storage Class in your Kubernetes cluster


## Identity Provider

The Bytewax Platform rely on an Identity Provider that implements OpenID Connect flow.
Lean more about the Bytewax Platform and the needed IdP configuration [here](setup/identity-provider)

## External Access to Kubernetes Cluster

The Bytewax Platform exposes two Kubernetes services: Dashboard UI and WaxAPI. Both need to receive requests from outside the Kubernetes cluster.
To configure the Bytewax Platform external access you can check all the details [here](setup/external-access)

## Default Storage Class

Normally a Kubernetes cluster has at least a Storace Class defined and that Storage Class is the default one. You can check easily if your cluster has that configured running this:

```bash
kubectl get sc
```

The desired output is something similar to this:
```
NAME                 PROVISIONER                RECLAIMPOLICY   VOLUMEBINDINGMODE   ALLOWVOLUMEEXPANSION   AGE
standard (default)   k8s.io/minikube-hostpath   Delete          Immediate           false                  84d
```

Where there is at least one entry and it has the `(default)` legend in its name.
