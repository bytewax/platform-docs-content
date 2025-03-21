The Bytewax Platform architecture is based on the [Kubernetes controller pattern](https://kubernetes.io/docs/concepts/architecture/controller/) applied to the [Dataflow Custom Resource](/reference/dataflow-crd).

At a high level, the Bytewax Platform consists of these components running on Kubernetes:

- Operator
- Dashboard
- WaxAPI
- Waxctl

The Operator is a core component running in the Kubernetes cluster. It implements the controller pattern over the Dataflow custom resources deployed on that cluster. Each time a Dataflow is created, modified or deleted, the Operator takes the needed actions to make the current state like the desired state.

```mermaid
graph LR;
  client([user])-. CRUD Dataflows<br>using waxctl, waxapi or dashboard .->ingress[Dataflow CR];
  ingress-->|events|service[Bytewax<br>Platform];
  subgraph Kubernetes cluster
  ingress;
  service-->pod1[Dataflow Pod 1];
  service-->pod2[Dataflow Pod 2];
  end
  classDef plain fill:#ddd,stroke:#fff,stroke-width:4px,color:#000;
  classDef k8s fill:#326ce5,stroke:#fff,stroke-width:4px,color:#fff;
  classDef cluster fill:#fff,stroke:#bbb,stroke-width:2px,color:#326ce5;
  classDef bw fill:#fab90f,stroke:#fff,stroke-width:2px,color:#fff;
  class ingress,pod1,pod2 k8s;
  class client plain;
  class cluster cluster;
  class service bw;
```

The Dashboard allows you to manage your Dataflows visually.

The WaxAPI is a RESTful API gateway responsible for receiving requests from clients and interacting with Kubernetes API.

Waxctl is the Bytewax CLI. It allows you to manage your Dataflows from the command-line.

## The Bytewax Platform Ecosystem

```mermaid
graph LR;
  client([user])
  waxctl[Waxctl]
  client-.terminal.->waxctl;
  waxctl --> k8sapi;
  client --> dashboard
  dashboard --> service
  idp(Identity<br>Provider) <--> |OpenID Connect<br>flow|service;
  idp <--> |OpenID Connect<br>flow|dashboard
  pod1 --> |metrics|prom[(Prometheus)];
  pod1 --> |traces|otel[(OpenTelemetry)];
  pod1 --> |recovery<br>snapshots|bucket[(S3)];
  subgraph Kubernetes cluster
  k8sapi[kubeAPI]
  pod1[Dataflow<br>stack]
  subgraph Bytewax Platform
  operator[Operator]
  service[WaxAPI]
  dashboard[Dashboard]
  end
  service --> k8sapi
  operator --> k8sapi
  k8sapi-->pod1;
  end
  classDef plain fill:#ddd,stroke:#fff,stroke-width:4px,color:#000;
  classDef k8s fill:#326ce5,stroke:#fff,stroke-width:4px,color:#fff;
  classDef cluster fill:#fff,stroke:#bbb,stroke-width:2px,color:#326ce5;
  classDef bw fill:#fab90f,stroke:#fff,stroke-width:2px,color:#fff;
  class ingress,pod1,k8sapi k8s;
  class client plain;
  class cluster cluster;
  class service,dashboard,operator,waxctl bw;
```


## Open ID Connect Integration

The Dashboard and WaxAPI are integrated with a third-party OpenID Connect provider to authenticate every request they receive. 

The Bytewax Platform can work with identity providers which implement authorization code flow with Proof Key for Code Exchange (PKCE).

