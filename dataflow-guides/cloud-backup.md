This guide will walk you through how to configure and test running a dataflow with cloud recovery. To do so we will start with an AWS kubernetes cluster and then migrate the same dataflow to a Gooogle Cloud Kubernetes cluster. Having an account with a cluster in each is a prerequisite for trying this.

Cloud recovery is a capability that allows the state persisted in the local worker to be written to object storage like s3. In the event of a node dying, the cluster going down or some other catastrophic failure, you will be able to recover dataflows from this bucket. This can also aid in Kubernetes cluster migration without the loss of state.

## Steps

1. Deploy a dataflow with recovery and cloud backup enabled

```sh
waxctl df deploy simple_slow.py \
  --platform \
  --name demo01 \
  --recovery=true \
  --recovery-backup=true \
  --recovery-backup-s3-url=s3://backup-tests/demo01 \
  --recovery-backup-s3-k8s-secret=aws-credentials
```

2. Show the state

You can view the dataflow and the state object via the platform dashboard.
[deployed dashboard](https://dashboard.lab.bytewax.io/)

3. Delete the dataflow

Now we can remove the dataflow to test the recovery. The `--yes` flag indicates that we would like to not do a dry run, which is the default behavior.

```sh
waxctl df rm --name demo01 --yes
```

4. Show that the dataflow was deleted using waxctl and/or the dashboard

We can verify the dataflow has been deleted by a waxctl command or via the dashboard UI.

```sh
waxctl df ls
```

or [click](https://dashboard.lab.bytewax.io/) to visit the dashboard.

5. Change kubectl configuration to use the GKE cluster

```sh
kubectl config use-context gke-01
```

6. Show that the dataflow `demo01` doesn't exist in the cluster

```sh
waxctl df ls -A
```

https://dashboard.gke-01.bytewax.io/

7. Deploy the dataflow again

```sh
waxctl df deploy simple_slow.py \
  --platform \
  --name demo01 \
  --recovery=true \
  --recovery-backup=true \
  --recovery-backup-s3-url=s3://litestream-tests/demo01 \
  --recovery-backup-s3-k8s-secret=aws-credentials
```

8. Show the state again

https://dashboard.gke-01.bytewax.io/
