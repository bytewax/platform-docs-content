Description of guide

## Steps

1. Deploy a dataflow with recovery and cloud backup enabled
```sh
waxctl df deploy simple_slow.py \
  --platform \
  --name demo01 \
  --recovery=true \
  --recovery-backup=true \
  --recovery-backup-s3-url=s3://litestream-tests/demo01 \
  --recovery-backup-s3-k8s-secret=aws-credentials
```

2. Show the state

https://dashboard.lab.bytewax.io/

3. Delete the dataflow
```sh
waxctl df rm --name demo01 --yes
```

4. Show that the dataflow is not anymore using waxctl and/or the dashboard
```sh
waxctl df ls
```
https://dashboard.lab.bytewax.io/

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
