## Waxctl and the Bytewax Platform

Waxctl is the Bytewax Command Line Interface. That tool helps you manage your dataflows in Kubernetes, AWS EC2, GCP VMs, and in your local machine.

That said, Waxctl works with open-source Bytewax dataflows or the Bytewax Platform dataflows.

## Deploy on the Platform

To deploy a dataflow to a Kubernetes cluster using Waxctl, you must run the `waxctl df deploy` command.

The following are special flags for that command only available on a Kubernetes cluster with the Bytewax Platform installed: 

```bash
--platform                                          deploy the dataflow as a bytewax.io/dataflow Custom Resource (requires Bytewax Platform installed)
--recovery-backup                                   back up worker state DBs to cloud storage (must have recovery flag present and provide S3 parameters)
--recovery-backup-s3-aws-access-key-id string       AWS credentials access key id
--recovery-backup-s3-aws-secret-access-key string   AWS credentials secret access key
--recovery-backup-s3-k8s-secret string              name of the Kubernetes secret storing AWS credentials.
--recovery-backup-s3-url string                     s3 url to store state backups. For example, s3://mybucket/mydataflow-state-backups
--recovery-single-volume                            use only one persistent volume for all dataflow's pods in Kubernetes
```
