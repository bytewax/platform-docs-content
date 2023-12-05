
## OpenID Connect and Bytewax Platform

OpenID Connect is a simple identity layer built on top of the OAuth 2.0 protocol, which allows clients to verify the identity of an end user based on the authentication performed by an authorization server or identity provider (IdP), as well as to obtain basic profile information about the end user in an interoperable and REST-like manner. OpenID Connect specifies a RESTful HTTP API, using JSON as a data format.

The Bytewax Platform requires an IdP implementing OpenID Connect to authenticate the users.

## Identity Provider setup

Depending on each IdP, the names could vary, but in general you will need to acomplish these:

1. Create Realm
2. Create Client in the new Realm
3. Create credentials for the new Client (you will need those for configuring your Bytewax Platform helm chart)
4. Configure URLs for the new Client
  - Root URL: `<YOUR Bytewax Platform Dashboard URL>`
  - Home URL: `/`
  - Valid redirect URIs: `/login`
  - Valid post logout redirect URIs: `/logout`
  - Web origins: `<YOUR Bytewax Platform Dashboard URL>`
5. Get the Auth Issuer URL.

Then, you need to complete these fields in the `oidc` block of your Bytewax Platform helm chart values file following one of these approachs:

### Creating a Kubernetes Secret to store the information

You need to create a Kubernetes secret to store OIDC information.

```bash
kubectl create secret generic my-oidc-config \           
--from-literal=auth-issuer=<YOUR IDP AUTH ISSUER URL> \         
--from-literal=client-id=<YOUR CLIENT ID> \
--from-literal=client-secret=<YOUR CLIENT SECRET> \               
--namespace bytewax-system
```

Ideally, you should use a secret management system like AWS Secrets Management to store that critical information and then use a tool in the Kubernetes cluster as External-Secrets to get the values from that system and store them in a Kubernetes Secret.

Regardless of how you created the Kubernetes Secret, you will need to configure your helm chart values like this:

```yaml
oidc:
  existingSecret: "my-oidc-config"
  authIssuerKey: "auth-issuer"
  clientIdKey: "client-id"
  clientSecretKey: "client-secret"
```

### Setting OIDC plain values in the Helm Chart values file

WARNING: This approach should not be used on production environments.

For simplicity, you can just set these fields on your helm chart values file:

```yaml
oidc:
  authIssuer: "<YOUR IDP AUTH ISSUER URL>"
  clientId: "<YOUR CLIENT ID>"
  clientSecret: "<YOUR CLIENT SECRET>"
```

## Auth Code Flow + PKCE

The Authorization Code Flow + PKCE is an OpenId Connect flow specifically designed to authenticate native or mobile application users.

Some Identity Providers only allow this flow for web applications requiring the creation of a SPA (e.g. Okta). For those IdPs, Bytewax Platform allows you to configure the OIDC settings like this:


Create a Kubernetes Secret to store Dashboard SPA client-id and client-secret:
```bash
kubectl create secret generic dashboard-pkce-data \
  --from-literal=client-id=<YOUR SPA CLIENT ID> \
  --from-literal=client-secret=<YOUR SPA CLIENT SECRET> \
  --namespace bytewax-system
```

Then, complete your helm chart values file like this:
```yaml
oidc:
  authIssuer: "<IDP AUTH ISSUER URL>"
  clientId: "<WEB APP CLIENT ID>"
  clientSecret: "<WEB APP CLIENT SECRET>"

dashboard:
  oidc:
    pkce: true
    existingSecret: "dashboard-pkce-data"
    clientIdKey: "client-id"
    clientSecretKey: "client-secret"
```


