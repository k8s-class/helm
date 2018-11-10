# Creating your own Helm Charts
```
    ############################################################################
    # Create your helm chart
    ############################################################################

    helm create web

    rm -rf web/templates/*.*

    rm -rf web/values.yaml


    ############################################################################
    # Chart.yaml
    ############################################################################

    * cd web

apiVersion: v1
appVersion: "1.0"
description: Apache helm chart
maintainers:
- email: jim.knott@thinkahead.com
  name: James Knott
  url: http://www.thinkahead.com
name: web
version: 0.2.0


    ############################################################################
    # Values.yaml
    ############################################################################

replicaCount: 1
nameSpace: sit

image:
  repository: 192.168.224.204:8123/ascena/ct-assets
  tag: sit
  pullPolicy: Always

container:
  restartPolicy: Always
  containerPort: 443

service:
  type: NodePort
  port: 443
  name: 443
  nodeport: 30307
  definition: web


nodeSelector: {}

tolerations: []

affinity: {}



    * cd into templates


    ############################################################################
    # NOTES.txt
    ############################################################################

Thank you for installing {{ .Chart.Name }}.

Your release is named {{ .Release.Name }}.

To learn more about the release, try:

  $ helm status {{ .Release.Name }}
  $ helm get {{ .Release.Name }}



    #############################################################################
    # service.yaml
    #############################################################################

---
apiVersion: v1
kind: Service
metadata:
  labels:
    io.kompose.service: {{ .Values.service.definition }}
  name: {{ .Values.service.definition }}
  namespace: {{ .Values.nameSpace }}
spec:
  ports:
  - name: {{ quote .Values.service.name }}
    port: {{ .Values.service.port }}
    nodePort: {{ .Values.service.nodeport }}
    targetPort: {{ .Values.service.port }}
  selector:
    io.kompose.service: {{ .Values.service.definition }}
  type: {{ .Values.service.type }}
status:
  loadBalancer: {}


    #############################################################################
    # Deployment.yaml
    #############################################################################

---
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  labels:
    io.kompose.service: {{ .Values.service.definition }}
  name: {{ .Values.service.definition }}
  namespace: {{ .Values.nameSpace }}
spec:
  replicas: {{ .Values.replicaCount }}
  strategy: {}
  template:
    metadata:
      creationTimestamp: null
      labels:
        io.kompose.service: {{ .Values.service.definition }}
    spec:
      containers:
      - image: "{{ .Values.image.repository }}:{{ .Values.image.tag }}"
        imagePullPolicy: {{ .Values.image.pullPolicy }}
        name: {{ .Chart.Name }}
        ports:
        - containerPort: {{ .Values.container.containerPort }}
      restartPolicy: {{ .Values.container.restartPolicy }}
      hostAliases:
      - ip: "127.0.0.1"
        hostnames:
         - "uitkube.catherines.com"
         - "uitkube.dressbarn.com"
         - "uitkube.maurices.com"
         - "uitkube.shopjustice.ascenaretail.com"
         - "uitkube.lanebryant.com"
status: {}

```
