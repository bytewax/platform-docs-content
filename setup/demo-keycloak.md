Local k8s cluster using included Keycloak for testing.

## Setup

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

```bash
echo "127.0.0.1 bytewax-platform-demokeycloak.bytewax-system.svc.cluster.local" >> /etc/hosts
```

```bash
helm upgrade --install bytewax-platform ./* -nbytewax-system -f ../values.yaml
```

## Running

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

Browse http://localhost:3000, use these credentials:

```
Username: bytewax
Password: bytewax
```

IMPORTANT: Keycloak will ask you to change the password the first time you log in.

## When to use this guide

This configuration should only be used for trying Bytewax Platform and because you don't have access to an existent IdP. 

This Keycloak installation is in development mode and also it isn't storing any information in a Persistent Volume on the Kubernetes cluster. That means if the Keycloak container re-starts, the password set to `bytewax` user will be `bytewax` again and Keycloak will ask you to change it again.

