This guide covers the basic process of installing the Bytewax Platform on Amazon EKS. If you are looking to run The Platform on any Kubernetes cluster, but using AWS as the license portal, you should follow the [AWS Marketplace running on EKS Anywhere Documentation](./aws-marketplace-anywhere).

You must have purchased a subscription for the Bytewax Platform in the [AWS Marketplace](https://aws.amazon.com/marketplace/pp/prodview-heqksqasqoy66)
otherwise the automation from this setup will fail with a missing entitlement error.

## Pre-requisites

To install the Bytewax Platform, you'll need to have the following tools installed:

- [awscli](https://aws.amazon.com/cli/)
- [kubectl](https://kubernetes.io/docs/tasks/tools/)
- [eksctl](https://eksctl.io/introduction/#installation)
- [helm](https://helm.sh/docs/intro/install/)

## Steps

After you have purchased your Bytewax Platform subscription, please be sure to click "Continue to Configuration", select the "Helm Chart" fulfillment option and click "Continue to Launch" to ensure that the AWS License Manager has been enabled for your AWS account.

1. Enable an IAM OIDC provider

Your Amazon EKS cluster needs to have an IAM OIDC provider enabled to set up IAM Roles for Service Accounts (IRSA). For guidance on setting up an IAM OIDC provider, see the AWS [documentation](https://docs.aws.amazon.com/eks/latest/userguide/enable-iam-roles-for-service-accounts.html) for an example of associating an IAM OIDC with your cluster.

2. Create the Bytewax Platform namespace in your Kubernetes cluster:

**_This naming convention will be used throughout, if you modify it, you will need to modify many of the commands in order for things to work_**

```bash
kubectl create namespace bytewax-system
```

3. Create the Kubernetes Service Account that the Bytewax Platform will use

Be sure to substitute your cluster name, and your cluster region in the following command:

```bash
eksctl create iamserviceaccount \
  --name bytewax-platform-controller-manager \
  --namespace bytewax-system \
  --cluster <YOUR_CLUSTER_NAME> \
  --attach-policy-arn arn:aws:iam::aws:policy/service-role/AWSLicenseManagerConsumptionPolicy \
  --approve \
  --override-existing-serviceaccounts \
  --region <YOUR_CLUSTER_REGION>
```

4. Bytewax Platform Helm Chart setup

Create a directory to store the Bytewax Platform helm chart.

```bash
mkdir awsmp-chart
cd awsmp-chart
```

Authenticate to the AWS registry.

```bash
aws ecr get-login-password \
  --region us-east-1 | helm registry login \
  --username AWS \
  --password-stdin 709825985650.dkr.ecr.us-east-1.amazonaws.com
```

Download the helm chart locally.

```bash
helm pull oci://709825985650.dkr.ecr.us-east-1.amazonaws.com/bytewax/platform --version 0.1.6
```

Extract the downloaded helm chart archive with the following command:
_If you do not have tar available, you will have to use another method to extract and the delete the compressed directory_

```bash
tar xf $(pwd)/* && find $(pwd) -maxdepth 1 -type f -delete
```

Create the helm chart values file `values.yaml` with the desired configuration in the same directory of `awsmp-chart` directory.
These values are optional configuration values to customize the installation of the Bytewax Platform.

The platform has additional configuration parameters outside of the defaults that you can learn more about [here](/setup/installation).

5. Deploy the Bytewax Platform

Deploy the helm chart to your EKS cluster with the following command from the `awsmp-chart` directory:

```bash
helm upgrade --install bytewax-platform ./platform -nbytewax-system -f ./values.yaml
```
