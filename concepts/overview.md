The Bytewax Platform allows you to manage and monitor your Dataflows.
TODO: complete using content from: https://www.bytewax.io/blog/platform

## How it works

The Platform runs on Kubernetes version 1.22 or superior and it represents the core of the following ecosystem:

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

