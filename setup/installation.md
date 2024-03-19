These setup instructions will help you prepare your environment before installing Bytewax Platform. It is assumed that you have a base understanding of Kubernetes networking and storage to complete these. If you need support, please contact the Bytewax team to guide you through setup and configuration.

## Prerequisites

The Bytewax Platform works as an ecosystem made up of the following components:

- Kubernetes cluster running version 1.22 or higher
- Identity Provider implementing the OpenID Connect flow
- External access network configured in your Kubernetes cluster (Ingress Controller, DNS server)
- Default Storage Class in your Kubernetes cluster

## Identity Provider

The Bytewax Platform relies on an Identity Provider that implements the OpenID Connect flow for authentication to the Bytewax Dashboard, and to authenticate the deployment of Dataflows with `waxctl`.

To learn more about the Bytewax Platform and the needed IdP configuration, click [here](/setup/identity-provider)

## External Access to Kubernetes Cluster

The Bytewax Platform exposes two Kubernetes services: the Dashboard UI and WaxAPI.

Both the Dashboard and the API need to be able to receive requests from outside the Kubernetes cluster
to be able to deploy and manage Dataflows.

To configure the Bytewax Platform for external access please refer to the
documentation [here](/setup/external-access).

## Default Storage Class

Normally a Kubernetes cluster has a default Storage Class defined. You can check to see if your cluster has a default Storage Class configured by running this command:

```bash
kubectl get sc
```

The desired output should be similar to this example:

```bash
NAME                 PROVISIONER                RECLAIMPOLICY   VOLUMEBINDINGMODE   ALLOWVOLUMEEXPANSION   AGE
standard (default)   k8s.io/minikube-hostpath   Delete          Immediate           false                  84d
```

There should be at least one entry and it should have the `(default)` designation in its name.

## Installation

Now that you have your environment properly configured, you can continue with the Platform installation. There are currently two possibilities for installation:

1. [Installation on Kubernetes anywhere via AWS licensing](./aws-marketplace-anywhere)
2. [Installation on EKS via AWS](./aws-marketplace-eks)
