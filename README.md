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
