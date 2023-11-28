## WaxAPI

```yaml
waxapi:
  allowedHosts: "*"
  port: 8080 # waxapi application port, it will be set to container port and service targetPort
  ingress:
    enabled: false
    # For Kubernetes >= 1.18 you should specify the ingress-controller via the field ingressClassName
    # See https://kubernetes.io/blog/2020/04/02/improvements-to-the-ingress-api-in-kubernetes-1.18/#specifying-the-class-of-an-ingress
    # ingressClassName: nginx
    annotations: {}
    labels: {}
    # Used to create an Ingress record.
    # hosts:
    #   - waxapi.yourdomain.com
    # annotations:
    #   kubernetes.io/ingress.class: nginx
    #   kubernetes.io/tls-acme: "true"
    # labels:
    #   name: value
    # tls:
    #   # Secrets must be manually created in the namespace.
    #   - secretName: waxapi-tls
    #     hosts:
    #       - waxapi.yourdomain.com
  service:
    annotations: {}
    labels: {}
    type: ClusterIP
    # List of IP ranges that are allowed to access the load balancer (if supported)
    loadBalancerSourceRanges: []
    # Specify a specific node port when type is NodePort
    # nodePort: 32500
    type: ClusterIP
    port: 8080
```

## Dashboard

```yaml
dashboard:
  apiUrl: "http://localhost:8080/"
  baseUrl: "http://localhost:3000/"
  ingress:
    enabled: false
    # For Kubernetes >= 1.18 you should specify the ingress-controller via the field ingressClassName
    # See https://kubernetes.io/blog/2020/04/02/improvements-to-the-ingress-api-in-kubernetes-1.18/#specifying-the-class-of-an-ingress
    # ingressClassName: nginx
    annotations: {}
    labels: {}
    # Used to create an Ingress record.
    # hosts:
    #   - dashboard.yourdomain.com
    # annotations:
    #   kubernetes.io/ingress.class: nginx
    #   kubernetes.io/tls-acme: "true"
    # labels:
    #   name: value
    # tls:
    #   # Secrets must be manually created in the namespace.
    #   - secretName: dashboard-tls
    #     hosts:
    #       - dashboard.yourdomain.com
  service:
    annotations: {}
    labels: {}
    type: ClusterIP
    # List of IP ranges that are allowed to access the load balancer (if supported)
    loadBalancerSourceRanges: []
    # Specify a specific node port when type is NodePort
    # nodePort: 32500
    port: 3000
```