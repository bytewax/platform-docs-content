## Waxctl and the Bytewax Platform

Waxctl is the Bytewax Command Line Interface. That tool helps you manage your dataflows in Kubernetes, AWS EC2, GCP VMs, and in your local machine.

That said, Waxctl works with open-source Bytewax dataflows or the Bytewax Platform dataflows.

## Deploy on the Platform

To deploy a dataflow to a Kubernetes cluster using Waxctl, you must run the `waxctl df deploy` command.

The following are special flags for that command only available on a Kubernetes cluster with the Bytewax Platform installed: 

```bash
      --concurrency-policy string                         specifies how to treat concurrent executions of a Scheduled Dataflow (the value must be Allow, Forbid or Replace) (default "Forbid")
      --platform                                          deploy the dataflow as a bytewax.io/dataflow Custom Resource (requires Bytewax Platform installed)
      --recovery                                          stores recovery files in Kubernetes persistent volumes to allow resuming after a restart (your dataflow must have recovery enabled: https://bytewax.io/docs/getting-started/recovery)
      --recovery-backup                                   back up worker state DBs to cloud storage (must have recovery flag present and provide S3 parameters)
      --recovery-backup-interval int                      System time duration in seconds to keep extra state snapshots around
      --recovery-backup-s3-aws-access-key-id string       AWS credentials access key id
      --recovery-backup-s3-aws-secret-access-key string   AWS credentials secret access key
      --recovery-backup-s3-k8s-secret string              name of the Kubernetes secret storing AWS credentials.
      --recovery-backup-s3-url string                     s3 url to store state backups. For example, s3://mybucket/mydataflow-state-backups
      --recovery-parts int                                number of recovery parts (default 1)
      --recovery-single-volume                            use only one persistent volume for all dataflow's pods in Kubernetes
      --recovery-size string                              size of the persistent volume claim to be assign to each dataflow pod in Kubernetes (default "10Gi")
      --recovery-snapshot-interval int                    system time duration in seconds to snapshot state for recovery
      --recovery-storageclass string                      storage class of the persistent volume claim to be assign to each dataflow pod in Kubernetes
      --schedule string                                   dataflow schedule in Cron format, see https://en.wikipedia.org/wiki/Cron
```
