The Bytewax Platform is designed to address many common problems with the management of Dataflows on Kubernetes.

In this section we will outline some of the features that are available as a part of the platform.

## Deployment

CI/CD Integration and deployment tooling using [waxctl]()

A Dashboard UI to monitor and manage Dataflows.

## Metrics, monitoring and Tracing

Bytewax Dataflows can be deployed with [OpenTelemetry](https://opentelemetry.io/) metrics and tracing,
which can be integrated with distributed tracing platforms like [Jaeger](https://www.jaegertracing.io/).

Bytewax Dataflows collect detailed metrics and expose [Prometheus](https://prometheus.io/) metrics collection
endpoints.

## Use your existing stack: Built-on K8s and leverages common open source projects

The Bytewax Platform is designed to integrate with existing Kubernetes installations, single-sign-on
providers and a wide range of existing Kubernetes infrastructure. It can be deployed on cloud-managed
platforms, like EKS or on-prem Kubernetes clusters.

## Disaster recovery, and rescaling out-of-the-box

The Bytewax Platform is designed to simplify the management of stateful workloads that require
redundancy, persistence and disaster recovery.

Stateful workload persistence is automatically managed by the Bytewax Platform when rescaling the number of
workers in a Dataflow.

## Keep it customizable and extensible: Comprehensive platform API, and UI customization

The Bytewax Platform is designed to be extensible through the Platform API, to enable integration
with existing systems like CI and CD.
