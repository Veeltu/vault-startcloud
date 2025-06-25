```
veeltu@host-veeltu:~$ microk8s helm show values prometheus-community/prometheus-snmp-exporter
restartPolicy: Always

kind: Deployment

image:
  repository: quay.io/prometheus/snmp-exporter
  # if not set appVersion field from Chart.yaml is used
  tag: ""
  pullPolicy: IfNotPresent

imagePullSecrets: []
nodeSelector: {}
tolerations: []
affinity: {}
topologySpreadConstraints: []

## Assign a PriorityClassName to pods if set
# priorityClassName: ""

## Provide a namespace to substitude for the namespace on resources
namespaceOverride: ""

## Security context to be added to snmp-exporter pods
securityContext: {}
  # fsGroup: 1000
  # runAsUser: 1000
  # runAsNonRoot: true

## Security context to be added to snmp-exporter containers
containerSecurityContext:
  runAsNonRoot: true
  runAsUser: 1000
  readOnlyRootFilesystem: true

## Additional labels to add to all resources
customLabels: {}
  # app: snmp-exporter

## Custom snmp-exporter configuration
# This will replace the container's default configuration but that is still available at /etc/snmp_exporter/snmp.yml and can be used in addition via `extraArgs`
# config: |-
#   auths:
#     custom_v1:
#       community: custom
#       version: 1

extraConfigmapMounts: []
  # - name: snmp-exporter-configmap
  #   mountPath: /run/secrets/snmp-exporter
  #   subPath: snmp.yaml # (optional)
  #   configMap: snmp-exporter-configmap-configmap
  #   readOnly: true
  #   defaultMode: 420

## Additional init containers
# These will be added to the prometheus-snmp-exporter pod.
extraInitContainers: []
  # - name: init-myservice
  #   image: busybox:1.28
  #   command: [ 'sh', '-c', "sleep 10; done" ]

## Additional secret mounts
# Defines additional mounts with secrets. Secrets must be manually created in the namespace.
extraSecretMounts: []
  # - name: secret-files
  #   mountPath: /run/secrets/snmp-exporter
  #   secretName: snmp-exporter-secret-files
  #   readOnly: true
  #   defaultMode: 420

# Additional volumes, e.g. for secrets used in an extraContainer
extraVolumes: []

# Additional volume mounts for snmp-exporter container
extraVolumeMounts: []

## For RBAC support:
rbac:
  # Specifies whether RBAC resources should be created
  create: true

serviceAccount:
  # Specifies whether a ServiceAccount should be created
  create: true

  # The name of the ServiceAccount to use.
  # If not set and create is true, a name is generated using the fullname template
  name:

resources: {}
  # limits:
  #   memory: 300Mi
  # requests:
  #   memory: 50Mi

livenessProbe:
  httpGet:
    path: /
    port: http
readinessProbe:
  httpGet:
    path: /
    port: http

service:
  annotations: {}
  type: ClusterIP
  port: 9116
  ipDualStack:
    enabled: false
    ipFamilies: ["IPv6", "IPv4"]
    ipFamilyPolicy: "PreferDualStack"

## An Ingress resource can provide name-based virtual hosting and TLS
## termination among other things for CouchDB deployments which are accessed
## from outside the Kubernetes cluster.
## ref: https://kubernetes.io/docs/concepts/services-networking/ingress/
ingress:
  enabled: false
  ## Class name can be set since version 1.18.
  className: ""
  ## Path type is required since version 1.18. Default: ImplementationSpecific.
  pathType: ""
  hosts: []
    # - chart-example.local
  annotations: {}
    # kubernetes.io/ingress.class: nginx
    # kubernetes.io/tls-acme: "true"
  tls: []
    # Secrets must be manually created in the namespace.
    # - secretName: chart-example-tls
    #   hosts:
    #     - chart-example.local

podAnnotations: {}

extraArgs: []
# - --history.limit=1000

envFrom: []
#  - secretRef:
#      name: name-of-secret

replicas: 1

# Uncomment to set a specific revisionHistoryLimit. Defaults to 10 as per Kubernetes API if not set.
# revisionHistoryLimit: 3

## Monitors ConfigMap changes and POSTs to a URL
## Ref: https://github.com/prometheus-operator/prometheus-operator/tree/main/cmd/prometheus-config-reloader
##
configmapReload:
  ## configmap-reload container name
  ##
  enabled: false
  name: configmap-reload

  extraArgs: []
  # - "--watched-dir=/etc/configuration"

  # Mounts extraVolumes specified for snmp-exporter pod. Can use projected volumes to mounts multiple configMaps into same directory
  extraVolumeMounts: []
  # - name: projeted-volume
  #   mountPath: "/etc/configuration"
  #   readOnly: true

  ## configmap-reload container image
  ##
  image:
    repository: quay.io/prometheus-operator/prometheus-config-reloader
    tag: v0.83.0
    pullPolicy: IfNotPresent

  ## configmap-reload resource requests and limits
  ## Ref: http://kubernetes.io/docs/user-guide/compute-resources/
  ##
  resources: {}

  ## configmap-reload container security context
  containerSecurityContext: {}
    # runAsNonRoot: true
    # runAsUser: 1000
    # readOnlyRootFilesystem: true

# Enable this if you're using https://github.com/prometheus-operator/prometheus-operator
# A service monitor will be created per each item in serviceMonitor.params[]
serviceMonitor:
  enabled: false
  # Default value is the namespace the release is deployed to
  # namespace: monitoring

  path: /snmp

  # Fall back to the prometheus default unless specified
  # interval: 10s
  scrapeTimeout: 10s
  module:
    - if_mib
  # Auth used for scraping.
  auth:
    - public_v2

  # Relabelings dynamically rewrite the label set of a target before it gets scraped.
  # Set if defined unless overriden by params.relabelings.
  # https://github.com/prometheus-operator/prometheus-operator/blob/main/Documentation/api-reference/api.md#relabelconfig
  relabelings: []
    # - sourceLabels: [__address__]
    #   targetLabel: __param_target
    # - sourceLabels: [__param_target]
    #   targetLabel: instance

  # Metric relabeling is applied to samples as the last step before ingestion.
  # Set if defined unless overriden by params.additionalMetricsRelabels.
  # This sets fixed relabel configs with action 'replace'.
  additionalMetricsRelabels: {}
    # targetLabel1: replacementValue1
    # targetLabel2: replacementValue2

  # Metric relabeling is applied to samples as the last step before ingestion.
  # Set if defined unless overridden by params.additionalMetricsRelabelConfigs.
  # This allows setting arbitrary relabel configs.
  # https://github.com/prometheus-operator/prometheus-operator/blob/main/Documentation/api-reference/api.md#relabelconfig
  additionalMetricsRelabelConfigs: []
    # - sourceLabels: [__name__]
    #   targetLabel: __name__
    #   action: replace
    #   regex: (.*)
    #   replacement: prefix_$1

  # Label for selecting service monitors as set in Prometheus CRD.
  # https://github.com/prometheus-operator/prometheus-operator/blob/main/Documentation/api-reference/api.md#prometheusspec
  selector:
    prometheus: kube-prometheus

  # Retain the job and instance labels of the metrics retrieved by the snmp-exporter
  # https://github.com/prometheus-operator/prometheus-operator/blob/main/Documentation/api-reference/api.md#endpoint
  honorLabels: true

  params: []
    # Human readable URL that will appear in Prometheus / AlertManager
    # - name: localhost
    # The target that snmp will scrape
    #   target: 127.0.0.1
    # Module used for scraping. Overrides value set in serviceMonitor.module
    #   module:
    #     - if_mib
    # Map of labels for ServiceMonitor. Overrides value set in serviceMonitor.selector
    #   labels: {}
          # release: kube-prometheus-stack
    # Scraping interval. Overrides value set in serviceMonitor.interval
    #   interval: 30s
    # Scrape timeout. Overrides value set in serviceMonitor.scrapeTimeout
    #   scrapeTimeout: 30s
    # Relabelings. Overrides value set in serviceMonitor.relabelings
    #   relabelings: []
    # Map of metric labels and values to add. Overrides value set in serviceMonitor.additionalMetricsRelabels
    # This sets fixed relabel configs with action 'replace'.
    #   additionalMetricsRelabels: {}
    # Metrics relabelings. Overrides value set in serviceMonitor.additionalMetricsRelabelConfigs
    #   additionalMetricsRelabelConfigs: []

  ## If true, a ServiceMonitor CRD is created for snmp-exporter itself
  ##
  selfMonitor:
    enabled: false
    additionalMetricsRelabels: {}
    additionalRelabeling: []
    labels: {}
    path: /metrics
    scheme: http
    tlsConfig: {}
    interval: 30s
    scrapeTimeout: 30s

# Extra manifests to deploy as an array
extraManifests: []
  # - |
  #   apiVersion: v1
  #   kind: ConfigMap
  #   metadata:
  #   labels:
  #     name: prometheus-extra
  #   data:
  #     extra-data: "value"

strategy:
  rollingUpdate:
    maxSurge: 1
    maxUnavailable: 0
  type: RollingUpdate

veeltu@host-veeltu:~$
```

2025-06-25T12:35:13.751Z        debug   Scrape failed   {"resource": {"service.instance.id": "40c7009c-d69c-48a7-9f66-08a305c31ad5", "service.name": "otelcol-contrib", "service.version": "0.128.0"}, "otelcol.component.id": "prometheus", "otelcol.component.kind": "receiver", "otelcol.signal": "metrics", "scrape_pool": "snmp", "target": "http://127.0.0.1:9116/snmp?module=paloalto_fw&module=system&module=panorama_cpu&target=192.168.0.100", "err": "Get \"http://127.0.0.1:9116/snmp?module=paloalto_fw&module=system&module=panorama_cpu&target=192.168.0.100\": dial tcp 127.0.0.1:9116: connect: connection refused"}

2025-06-25T12:35:13.751Z        warn    internal/transaction.go:150     Failed to scrape Prometheus endpoint    {"resource": {"service.instance.id": "40c7009c-d69c-48a7-9f66-08a305c31ad5", "service.name": "otelcol-contrib", "service.version": "0.128.0"}, "otelcol.component.id": "prometheus", "otelcol.component.kind": "receiver", "otelcol.signal": "metrics", "scrape_timestamp": 1750854913749, "target_labels": "{__name__=\"up\", instance=\"192.168.0.100\", job=\"snmp\"}"}


2025-06-25T12:39:40.950Z        debug   Scrape failed   {"resource": {"service.instance.id": "da0b3e10-0e7a-45f6-9369-f045db0e2865", "service.name": "otelcol-contrib", "service.version": "0.128.0"}, "otelcol.component.id": "prometheus", "otelcol.component.kind": "receiver", "otelcol.signal": "metrics", "scrape_pool": "snmp", "target": "http://127.0.0.1:9116/snmp?module=paloalto_fw&module=system&module=panorama_cpu&target=192.168.0.100", "err": "Get \"http://127.0.0.1:9116/snmp?module=paloalto_fw&module=system&module=panorama_cpu&target=192.168.0.100\": dial tcp 127.0.0.1:9116: connect: connection refused"}

2025-06-25T12:39:40.951Z        warn    internal/transaction.go:150     Failed to scrape Prometheus endpoint    {"resource": {"service.instance.id": "da0b3e10-0e7a-45f6-9369-f045db0e2865", "service.name": "otelcol-contrib", "service.version": "0.128.0"}, "otelcol.component.id": "prometheus", "otelcol.component.kind": "receiver", "otelcol.signal": "metrics", "scrape_timestamp": 1750855180950, "target_labels": "{__name__=\"up\", instance=\"192.168.0.100\", job=\"snmp\"}"}