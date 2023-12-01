The Bytewax Platform is a framework for deploying and managing Bytewax Dataflows on Kubernetes.

The Platform can be deployed on a Kubernetes cluster running version 1.22 or higher, and is composed of the following components:

```mermaid
graph LR;
  client([user])-.waxctl on terminal.->service[Bytewax<br>Platform];
  client <--> Dashboard
  Dashboard <--> service
  idp(Identity<br>Provider) <--> |OpenID Connect<br>flow|service;
  idp <--> |OpenID Connect<br>flow|Dashboard
  pod1 --> |traces|otel(OpenTelemetry);
  pod1 --> |recovery<br>snapshots|bucket(S3);
  subgraph Kubernetes cluster
  service-->pod1[Dataflow<br>stack];
  end
  classDef plain fill:#ddd,stroke:#fff,stroke-width:4px,color:#000;
  classDef k8s fill:#326ce5,stroke:#fff,stroke-width:4px,color:#fff;
  classDef cluster fill:#fff,stroke:#bbb,stroke-width:2px,color:#326ce5;
  classDef bw fill:#fab90f,stroke:#fff,stroke-width:2px,color:#fff;
  class ingress,pod1 k8s;
  class client plain;
  class cluster cluster;
  class service,Dashboard bw;
```

## Feature Overview

The following section is an overview of the features offered by the Bytewax Platform.

### Management UI

The Bytewax Platform provides a web-based management UI, secured by single sign-on. Running Dataflows
can be monitored, stopped and started from the UI.

### Deployment

CI/CD Integration and deployment tooling using `waxctl`, a command line tool for managing dataflows.
For more information about waxctl, see the documentation [here](/reference/waxctl-for-platform).

### Metrics, monitoring and Tracing

Bytewax Dataflows can be deployed with [OpenTelemetry](https://opentelemetry.io/) metrics and tracing,
which can be integrated with distributed tracing platforms like [Jaeger](https://www.jaegertracing.io/).

Bytewax Dataflows collect detailed metrics and exposes a [Prometheus](https://prometheus.io/) metrics collection
endpoint.

### Use your existing stack: Built-on K8s and leverages common open source projects

The Bytewax Platform is designed to integrate with existing Kubernetes installations, single-sign-on
providers and a wide range of existing Kubernetes infrastructure. It can be deployed on cloud-managed
platforms, like EKS or on-prem Kubernetes clusters.

### Disaster recovery and rescaling

The Bytewax Platform is designed to simplify the management of stateful workloads that require
redundancy, persistence and disaster recovery.

Stateful workload persistence is automatically managed by the Bytewax Platform when rescaling the number of
workers in a Dataflow.

### Customizable and extensible

The Bytewax Platform is designed to be extensible through the Platform API, to enable integration
with existing systems for management and programatic deploys of dataflows.

The Bytewax Platform UI can be customized or to match your brand identity.

