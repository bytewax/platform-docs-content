## Pre-requisites

You must create a subcription in the [AWS Marketplace](https://aws.amazon.com/marketplace/pp/prodview-heqksqasqoy66) for the Bytewax Platform else the automation from this setup won't work due to a missing entitlement.

Additionally:

- [awscli](https://aws.amazon.com/cli/)
- [kubectl](https://kubernetes.io/docs/tasks/tools/)
- [eksctl](https://eksctl.io/introduction/#installation)
- [helm](https://helm.sh/docs/intro/install/)

## Steps

Your Amazon EKS cluster needs to have IAM OIDC provider enabled to set up IRSA. Learn more with AWS' Creating an IAM OIDC provider for your cluster doc:

https://docs.aws.amazon.com/eks/latest/userguide/enable-iam-roles-for-service-accounts.html


1. Setup needed variables:

```bash
$ cluster_name=<CLUSTER NAME>
$ cluster_region=<CLUSTER REGION>
```

2. Create Bytewax Platform namespace in your Kubernetes cluster:

```bash
$ kubectl create namespace bytewax-system
```

3. Create the Kubernetes Service Account that Bytewax Platform needs

```bash
$ eksctl create iamserviceaccount \
    --name bytewax-platform-controller-manager \
    --namespace bytewax-system \
    --cluster $cluster_name \
    --attach-policy-arn arn:aws:iam::aws:policy/service-role/AWSLicenseManagerConsumptionPolicy \
    --approve \
    --override-existing-serviceaccounts \
    --region $cluster_region
```

4. Bytewax Platform Helm Chart setup

```bash
mkdir awsmp-chart
cd awsmp-chart
```

```bash
aws ecr get-login-password \
    --region us-east-1 | helm registry login \
    --username AWS \
    --password-stdin 709825985650.dkr.ecr.us-east-1.amazonaws.com
```

```bash
helm pull oci://709825985650.dkr.ecr.us-east-1.amazonaws.com/bytewax/platform --version 0.1.5
```

```bash
tar xf $(pwd)/* && find $(pwd) -maxdepth 1 -type f -delete
```

Create helm chart values file `values.yaml` with the desired configuration in the same directory of `awsmp-chart` directory.

To learn more about Bytewax Platform options, please check [here](setup/installation).


5. Deploy Bytewax Platform

```bash
helm upgrade --install bytewax-platform ./* -nbytewax-system -f ../values.yaml
```


