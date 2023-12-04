The Bytewax Platform exposes two services outside the Kubernetes cluster:

- Dashboard Web App
- WaxAPI

This documentation shows you how to configure the Bytewax Platform Helm Chart to enable external access to both services.

## Common Configuration

This section describes the common configuration of Dashboard and WaxAPI.

### External Access through Ingress Controller

If your cluster has configured an Ingress Controller, you should configure this subblock in `waxapi` and `dashboard` block:

```yaml
  ingress:
    enabled: false
    # For Kubernetes >= 1.18 you should specify the ingress-controller via the field ingressClassName
    # See https://kubernetes.io/blog/2020/04/02/improvements-to-the-ingress-api-in-kubernetes-1.18/#specifying-the-class-of-an-ingress
    # ingressClassName: nginx
    annotations: {}
    labels: {}
    hosts:
      - waxapi.yourdomain.com
    annotations:
      kubernetes.io/ingress.class: nginx
      kubernetes.io/tls-acme: "true"
    labels:
      name: value
    tls:
      # Secrets must be manually created in the namespace.
      - secretName: waxapi-tls
        hosts:
          - waxapi.yourdomain.com
```

### External Access through External Load Balancer

In case you need to use a Kubernetes service of type `LoadBalancer` or `NodePort` to enable external access, you can configure the subblock `service` of `waxapi` and `dashboard` like this:

*LoadBalancer*

```yaml
waxapi:
  service:
    annotations: {}
    labels: {}
    type: LoadBalancer
    # List of IP ranges that are allowed to access the load balancer (if supported)
    loadBalancerSourceRanges: []
    port: 8080
```

*NodePort*

```yaml
  service:
    annotations: {}
    labels: {}
    type: NodePort
    # Specify a specific node port when type is NodePort
    nodePort: 32500
    port: 8080
```

Depending on the underlying cloud provider, the needed configuration for DNS and SSL will vary. 

Commonly, those configurations are set in annotations, which are available to place in the block, as you can see.

## Dashboard Exclusive Configuration

The `dashboard` block has two exclusive fields to configure in its block:

- apiUrl: stores the URL where WaxAPI is exposed. Following the below example, it will be: `https://waxapi.yourdomain.com/`
- baseUrl: stores the URL where the Dashboard itself is exposed. For example: `https://dashboard.yourdomain.com/`
