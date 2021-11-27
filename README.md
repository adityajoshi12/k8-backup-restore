# Kubernetes Backup and Restore


[![IMAGE ALT TEXT](http://img.youtube.com/vi/_y0yGAbLknU/0.jpg)](http://www.youtube.com/watch?v=_y0yGAbLknU "Video Tutorial")


1. Velero CLI Installation

```bash
wget https://github.com/vmware-tanzu/velero/releases/download/v1.7.0/velero-v1.7.0-linux-amd64.tar.gz

tar -xvf velero-v1.7.0-linux-amd64.tar.gz -C /tmp
sudo mv /tmp/velero-v1.7.0-linux-amd64/velero /usr/local/bin

velero version
```

1. AWS Credentails Setup

```bash
[default]
aws_access_key_id=
aws_secret_access_key=
```

1. Velero installation to K8

```bash
export BUCKET=k8-backup
export REGION=ap-south-1
velero install \
    --provider aws \
    --plugins velero/velero-plugin-for-aws:v1.3.0 \
    --bucket $BUCKET \
    --backup-location-config region=$REGION \
    --snapshot-location-config region=$REGION \
    --secret-file ./credentials-velero
```

```bash
kubectl get pods -n velero
```

Creating application

```bash
kubectl create deployment testing --image=nginx --replicas=2
kubectl expose deployment testing --name=test-srv --type=NodePort --port=80
kubectl port-forward svc/test-srv 8000:80
```

```bash
velero backup-location get
```

```bash
velero backup create cluster-backup
```

```bash
velero backup-location get
```

```bash
velero restore create new-backup-restore --from-backup new-backup
```

```bash
velero schedule create every5min --schedule="*/5 * * * *" --include-namespaces test --ttl 0h15m0s
```
