# helm
How to use helm and create charts
    ###################################################################################################
    # Helm Commands
    ###################################################################################################

    helm search local

    helm lint <chartname>

    helm package <chartname>

    helm install stable/drupal --set image=my-registry/drupal:0.1.0 --set livenessProbe.exec.command=[cat,docroot/CHANGELOG.txt] --set livenessProbe.httpGet=null

    helm upgrade --install <release name> --values <values file> <chart directory>

    helm install --name web web-0.1.0.tgz --set image.tag=development

    helm install --name web web-0.1.0.tgz --set image.tag=development --namespace sit

    helm upgrade --install web web-0.2.0.tgz --set image.tag=production --namespace sit

    helm upgrade --install web web-0.2.0.tgz --set image.tag=production --set nameSpace=production --namespace production

    helm install --name wlsadmin wlsadmin-0.1.0.tgz --namespace sit

    helm rollback web 1 --dry-run

    helm history web




    Setting up a Helm Repository on S3

    ./setup-s3-helm-repo.sh


# Helm

## Install helm
```
wget https://storage.googleapis.com/kubernetes-helm/helm-v2.9.1-linux-amd64.tar.gz
tar -xzvf helm-v2.9.1-linux-amd64.tar.gz
sudo mv linux-amd64/helm /usr/local/bin/helm
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
