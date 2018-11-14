
# Helm

## Install helm
```
wget https://storage.googleapis.com/kubernetes-helm/helm-v2.9.1-linux-amd64.tar.gz
tar -xzvf helm-v2.9.1-linux-amd64.tar.gz
sudo mv linux-amd64/helm /usr/local/bin/helm
```
# Windows Helm Version
```
https://storage.googleapis.com/kubernetes-helm/helm-v2.11.0-windows-amd64.zip
```

## Initialize helm

```
kubect create -f helm-rbac.yaml
helm init --service-account tiller
```

## Setup S3 helm repository
Make sure to set the default region in setup-s3-helm-repo.sh
```
./setup-s3-helm-repo.sh
```

## Package and push demo-chart

```
export AWS_REGION=yourregion 
helm package yourchartname
helm s3 push ./chart-0.0.1.tgz my-charts
