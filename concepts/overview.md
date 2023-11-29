The Bytewax Platform is a framework for deploying and managing Bytewax Dataflows on Kubernetes.

TODO: complete using content from: https://www.bytewax.io/blog/platform

## How it works

The Bytewax Platform runs on Kubernetes version 1.22 or higher, and is composed of the following components:

diagram showing:
- client side: 
    user -> waxctl, dashboard UI
    app  -> waxapi
- server side:
    identity provider (OIDC)
    bytewax platform                                       |
    dataflows                                              | --> k8s cluster
    o11y: otel collector + prometheus + jaeger + grafana   |
    Disaster Recovery: S3

TODO: Description of an end-to-end lifecycle example mentioning OIDC integration

