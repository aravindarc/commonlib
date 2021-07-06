# CommonLib: The Kubernetes Library Chart

This chart is designet to reuse the common components like deployment, service, etc.

# How to use the commonlib library chart in your helm chart

1. Include the commonlib chart as dependency in your `Chart.yaml` like this:
```yaml
dependencies:
- name: commonlib
  version: 0.1.0
  repository: <commonlib chart repo url>
```
2. Run:
```bash
helm dependency update mychart/
```
3. You can import templates in library chart like this:
```yaml
{{- include "commonlib.deployment" (list . "mychart.deployment") -}}
{{- define "mychart.deployment" -}}
# your override values
# can be empty
{{- end -}}
```
4. You can override template keys in library chart like this:
```yaml
{{- include "commonlib.serviceAccount" (list . "mychart.serviceAccount") -}}
{{- define "mychart.serviceAccount" -}}
{{- with .Values.serviceAccount.annotations }}
metadata:
  annotations:
    {{- toYaml . | nindent 4 }}
{{- end }}
{{- end -}}
```
5. Your values file should look like this:
```yaml
# Default values for mychart.
# This is a YAML-formatted file.
# Declare variables to be passed into your templates.

replicaCount: 1

image:
  repository: nginx
  pullPolicy: IfNotPresent
  # Overrides the image tag whose default is the chart appVersion.
  tag: ""

imagePullSecrets: []
nameOverride: ""
fullnameOverride: ""

serviceAccount:
  # Specifies whether a service account should be created
  create: true
  # Annotations to add to the service account
  annotations: {}
  # The name of the service account to use.
  # If not set and create is true, a name is generated using the fullname template
  name: ""

podAnnotations: {}

podSecurityContext: {}
  # fsGroup: 2000

securityContext: {}
  # capabilities:
  #   drop:
  #   - ALL
  # readOnlyRootFilesystem: true
  # runAsNonRoot: true
  # runAsUser: 1000

service:
  type: ClusterIP
  port: 80

resources: {}
  # We usually recommend not to specify default resources and to leave this as a conscious
  # choice for the user. This also increases chances charts run on environments with little
  # resources, such as Minikube. If you do want to specify resources, uncomment the following
  # lines, adjust them as necessary, and remove the curly braces after 'resources:'.
  # limits:
  #   cpu: 100m
  #   memory: 128Mi
  # requests:
  #   cpu: 100m
  #   memory: 128Mi

autoscaling:
  enabled: true
  minReplicas: 1
  maxReplicas: 100
  targetCPUUtilizationPercentage: 80
  # targetMemoryUtilizationPercentage: 80

nodeSelector: {}

tolerations: []

affinity: {}
```

# Kubernetes kinds currently supported
1. Deployment
2. Service
3. ServiceAccount
4. HPA