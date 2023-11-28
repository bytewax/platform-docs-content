## Pre-requisites

You must create a subcription in the [AWS Marketplace](https://aws.amazon.com/marketplace/pp/prodview-heqksqasqoy66) for the Bytewax Platform else the automation from this setup won't work due to a missing entitlement.

Additionally:

- [awscli](https://aws.amazon.com/cli/)
- [kubectl](https://kubernetes.io/docs/tasks/tools/)
- [helm](https://helm.sh/docs/intro/install/)

## Steps

1. Set variables with values obtained from AWS Marketplace subscription wizard:

AWSMP_TOKEN=<CREATED_TOKEN_ABOVE>
AWSMP_ROLE_ARN=<CREATED_ROLE_ABOVE>

2. Create Bytewax Platform namespace in your Kubernetes cluster:

```bash
$ kubectl create namespace bytewax-system
```

3. Create Kubernetes secret needed to check AWS Marketplace subscription:

```bash
kubectl create secret generic awsmp-license-token-secret \
--from-literal=license_token=$AWSMP_TOKEN \
--from-literal=iam_role=$AWSMP_ROLE_ARN \
--namespace bytewax-system
```

4. Create Kubernetes secret needed to pull Bytewax Platform docker images.

You need an active session with AWS in your terminal to run the following steps.

```bash
AWSMP_ACCESS_TOKEN=$(aws license-manager get-access-token \
    --output text --query '*' --token $AWSMP_TOKEN --region us-east-1)
```
```bash
AWSMP_ROLE_CREDENTIALS=$(aws sts assume-role-with-web-identity \
                --region 'us-east-1' \
                --role-arn $AWSMP_ROLE_ARN \
                --role-session-name 'AWSMP-guided-deployment-session' \
                --web-identity-token $AWSMP_ACCESS_TOKEN \
                --query 'Credentials' \
                --output text)
```
```bash
export AWS_ACCESS_KEY_ID=$(echo $AWSMP_ROLE_CREDENTIALS | awk '{print $1}' | xargs)
export AWS_SECRET_ACCESS_KEY=$(echo $AWSMP_ROLE_CREDENTIALS | awk '{print $3}' | xargs)
export AWS_SESSION_TOKEN=$(echo $AWSMP_ROLE_CREDENTIALS | awk '{print $4}' | xargs)
```

```bash
kubectl create secret docker-registry awsmp-image-pull-secret \
--docker-server=709825985650.dkr.ecr.us-east-1.amazonaws.com \
--docker-username=AWS \
--docker-password=$(aws ecr get-login-password --region us-east-1) \
--namespace bytewax-system
```

Those credentials expire in a few hours. If you need to reinstall the Bytewax Platform, and for some reason Kubernetes needs to pull the images again (e.g. running a pod in a different node), you will need to delete the Secret `awsmp-image-pull-secret` and repeat the below commands of the step 4 to have active docker credentials.


5. Bytewax Platform Helm Chart setup

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

Related to this guide, you must to set this value in your yaml file:

```yaml
billing:
  awsMarketplace:
    licenseConfigSecretName: awsmp-license-token-secret
```

There are other settings that you must fill before deploy the Bytewax Platform helm chart.
To learn more about it, please check [here](setup/installation).


6. Deploy Bytewax Platform

```bash
helm upgrade --install bytewax-platform ./* -nbytewax-system -f ../values.yaml
```
