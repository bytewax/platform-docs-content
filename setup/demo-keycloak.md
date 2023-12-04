These setup instructions cover how to install the Bytewax Platform with a demonstration Keycloak installation. These instructions should only be used to configure the Bytewax Platform for development or demonstration purposes.

**Please note**- the example configuration of Keycloak provided here should be considered **insecure** and **NOT INTENDED** for use in production.

## Setup

Before deploying, create a `values.yaml` file with the following contents verbatim:

```bash
cat << EOF > ../values.yaml
oidc:
  authIssuer: "http://bytewax-platform-demokeycloak.bytewax-system.svc.cluster.local:8880/realms/bytewax"
  clientId: "bytewax-platform"
  clientSecret: "**********"

demokeycloak:
  enabled: true
EOF
```

In order to access the Bytewax Platform using port-forwarding, we'll need to add an entry our hosts file:

```bash
echo "127.0.0.1 bytewax-platform-demokeycloak.bytewax-system.svc.cluster.local" >> /etc/hosts
```

Now that we have our values configured and our hostname set, we can deploy the platform.

```bash
helm upgrade --install bytewax-platform ./platform -nbytewax-system -f ./values.yaml
```

## Accessing the cluster

First, we'll need to establish some port forwarding that will allow us to access the cluster
via the dashboard, and to communicate with it via `waxctl`.

### Terminal 1
```bash
kubectl port-forward svc/bytewax-platform-waxapi 8080
```

### Terminal 2
```bash
kubectl port-forward svc/bytewax-platform-demokeycloak 8880
```

### Terminal 3
```bash
kubectl port-forward svc/bytewax-platform-dashboard 3000
```

Connect to http://localhost:3000 in your web browser and use these credentials to log in:

```
Username: bytewax
Password: bytewax
```

Keycloak will ask you to change the password the first time you log in.

## Important Notes

This Keycloak installation is deployed in development mode and isn't storing
any information in a Persistent Volume on the Kubernetes cluster.

If the Keycloak container re-starts, the password set to `bytewax` user will be
`bytewax` again and Keycloak will ask you to change it again.

