This guide covers the basic process of installing the Bytewax Platform on Amazon EKS Anywhere.

You must have purchased a subcription for the Bytewax Platform in the [AWS Marketplace](https://aws.amazon.com/marketplace/pp/prodview-heqksqasqoy66)
otherwise the automation from this setup will fail with a missing entitlement error.

## Pre-requisites

To install the Bytewax Platform, you'll need to have the following tools installed:

- [awscli](https://aws.amazon.com/cli/)
- [kubectl](https://kubernetes.io/docs/tasks/tools/)
- [helm](https://helm.sh/docs/intro/install/)

## Steps

1. Set environment variables with values obtained from AWS Marketplace subscription wizard:

```bash
AWSMP_TOKEN=<CREATED_TOKEN_ABOVE>
AWSMP_ROLE_ARN=<CREATED_ROLE_ABOVE>
```

2. Create the Bytewax Platform namespace in your Kubernetes cluster:

```bash
kubectl create namespace bytewax-system
```

3. Create Kubernetes secret needed to check AWS Marketplace subscription:

```bash
kubectl create secret generic awsmp-license-token-secret \
--from-literal=license_token=$AWSMP_TOKEN \
--from-literal=iam_role=$AWSMP_ROLE_ARN \
--namespace bytewax-system
```

4. Create the Kubernetes secret needed to pull Bytewax Platform docker images.

You need an authenticated session with AWS in your terminal to run the following `awscli` steps.

First, let's retrieve our AWS License manager access token.

```bash
AWSMP_ACCESS_TOKEN=$(aws license-manager get-access-token \
    --output text --query '*' --token $AWSMP_TOKEN --region us-east-1)
```

With that token, we can fetch AWS role credentials that we'll use in our cluster.

```bash
AWSMP_ROLE_CREDENTIALS=$(aws sts assume-role-with-web-identity \
                --region 'us-east-1' \
                --role-arn $AWSMP_ROLE_ARN \
                --role-session-name 'AWSMP-guided-deployment-session' \
                --web-identity-token $AWSMP_ACCESS_TOKEN \
                --query 'Credentials' \
                --output text)
```

AWS will return an Access Key ID, a Secret Access Key, and a Session token together in the last call.
We can break those out into their own environment variables with the following commands:

```bash
export AWS_ACCESS_KEY_ID=$(echo $AWSMP_ROLE_CREDENTIALS | awk '{print $1}' | xargs)
export AWS_SECRET_ACCESS_KEY=$(echo $AWSMP_ROLE_CREDENTIALS | awk '{print $3}' | xargs)
export AWS_SESSION_TOKEN=$(echo $AWSMP_ROLE_CREDENTIALS | awk '{print $4}' | xargs)
```

The following command will create a secret in Kubernetes that can be used to pull the required Bytewax container images
from the AWS registry.

```bash
kubectl create secret docker-registry awsmp-image-pull-secret \
--docker-server=709825985650.dkr.ecr.us-east-1.amazonaws.com \
--docker-username=AWS \
--docker-password=$(aws ecr get-login-password --region us-east-1) \
--namespace bytewax-system
```

Those credentials expire in a few hours. If you need to reinstall the Bytewax
Platform, and for some reason Kubernetes needs to pull the images again
(e.g. running a pod in a different node), you will need to delete the Secret
`awsmp-image-pull-secret` and repeat the commands of the step 4 to have
active docker credentials.

5. Bytewax Platform Helm Chart setup

Now that we have our environment prepared, we can prepare the Bytewax Platform Helm chart for installation.

First, we'll create a new directory for the Bytewax Platform Helm chart.

```bash
mkdir awsmp-chart
cd awsmp-chart
```

Next, we'll need to authenticate to the AWS registry:

```bash
aws ecr get-login-password \
    --region us-east-1 | helm registry login \
    --username AWS \
    --password-stdin 709825985650.dkr.ecr.us-east-1.amazonaws.com
```

Now that we're authenticated, we can download the chart with `helm pull`.

```bash
helm pull oci://709825985650.dkr.ecr.us-east-1.amazonaws.com/bytewax/platform --version 0.1.5
```

Unzip the downloaded helm chart archive with the following command:

```bash
tar xf $(pwd)/* && find $(pwd) -maxdepth 1 -type f -delete
```

Create helm chart values file `values.yaml` with the desired configuration in the same directory of `awsmp-chart` directory.

For installations of the Bytewax Helm chart in EKS Anywhere, we need to add the following values to `values.yaml`.

```yaml
billing:
  awsMarketplace:
    licenseConfigSecretName: awsmp-license-token-secret
```

The values.yaml contains additional configuration options that are needed to deploy the Bytewax Platform helm chart.
To learn more about available configuration options, please check [here](setup/installation).


6. Deploy Bytewax Platform

Now that we've configured our helm chart installation, we can proceed with installing it. This
command should be run from the `awsmp-chart` directory.

```bash
$ helm upgrade --install bytewax-platform ./platform -nbytewax-system -f ./values.yaml
```
