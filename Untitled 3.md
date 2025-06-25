```
values.yaml
 
 service:
type: ClusterIP
port: 9116

extraArgs:
- --config.file=/snmp.yml
#  - --config.expand-environment-variables

 
 
snmp.yml
 
 
 auths:
paloalto:
   community: public


modules:
panorama_cpu:
   walk:
     - 1.3.6.1.2.1.25.3  # hrProcessorLoad
   metrics:
     - name: hrProcessorLoad
       oid: 1.3.6.1.2.1.25.3.3.1.2
       type: gauge
       help: "Average CPU load"

 
obs.yaml

 
 mode: "deployment"
resources:
limits:
   cpu: 2000m
   memory: 2Gi
requests:
   cpu: 500m
   memory: 750Mi
image:
repository: "europe-west3-docker.pkg.dev/nz-mgmt-shared-artifacts-8c85/ghcr-io/open-telemetry/opentelemetry-collector-releases/opentelemetry-collector-contrib"
pullPolicy: IfNotPresent
tag: "0.125.0"
ports:
syslog-rfc5424:
   enabled: true
   containerPort: 54526
   servicePort: 54526
   hostPort: 54526
   protocol: TCP
extraEnvs:
- name: MY_POD_IP
   valueFrom:
     fieldRef:
       apiVersion: v1
       fieldPath: status.podIP
- name: KAFKA_USERNAME
   value: "poc-otel-nsd-security-rw"
- name: KAFKA_PASSWORD
   valueFrom:
     secretKeyRef:
       key: client-passwords
       name: kafka-client-pwd-poc-otel-nsd-security-rw
       optional: false
config:
receivers:
   syslog:
     tcp:
       listen_address: ${env:MY_POD_IP}:54526
     protocol: rfc5424
     enable_octet_counting: false
     allow_skip_pri_header: false
     location: UTC
   prometheus/paloalto:
     config:
       scrape_configs:
         - job_name: snmp_panorama
           static_configs:
             - targets: 
               - 10.89.37.245
#                - 10.89.64.116
           relabel_configs:
             - source_labels: [__address__]
               target_label: __param_target
             - source_labels: [__param_target]
               target_label: instance
             - target_label: __address__
               replacement:  snmp-exporter-prometheus-snmp-exporter.security.svc.cluster.local:9116
           metrics_path: /snmp
           params:
             auth: 
               - paloalto
             module:
               - paloalto_fw
               - system
               - panorama_cpu
#          - job_name: snmp_fw
#            static_configs:
#              - targets: 
#                - 100.126.0.167
#                - 100.126.2.166
#                - 100.126.116.52
#                - 100.126.0.167
#                - 100.126.0.168
#                - 100.126.0.166
#                - 100.126.0.165
#                - 100.126.112.102
#                - 100.126.1.168
#                - 100.126.1.167
#                - 100.126.1.166
#                - 100.126.1.165
#                - 100.126.5.5
#            relabel_configs:
#              - source_labels: [__address__]
#                target_label: __param_target
#              - source_labels: [__param_target]
#                target_label: instance
#              - target_label: __address__
#                replacement:  snmp-exporter-prometheus-snmp-exporter.security.svc.cluster.local:9116
#            metrics_path: /snmp
#            params:
#              auth: []
#              module:
#                - paloalto_fw

exporters:
   kafka/metrics:
     brokers:
       - otel-kafka-bootstrap.kafka-cluster:9094
     producer:
       max_message_bytes: 2000000
       compression: zstd
     protocol_version: 3.9.0
     topic: poc-otel-security-metrics
     auth:
       sasl:
         username: ${env:KAFKA_USERNAME} 
         password: ${env:KAFKA_PASSWORD} 
         mechanism: SCRAM-SHA-512
         version: 1 
service:
   pipelines:
     metrics:
       receivers:
        - otlp
        - prometheus
        - prometheus/paloalto   
       exporters:
        - debug
        - kafka/metrics
       processors:
        - memory_limiter
        - batch
     logs:
       receivers: [syslog, otlp]
       processors: 
        - memory_limiter
        - batch
       exporters: [debug]
```


2025-06-25T11:52:59.155Z        debug   Scrape failed   {"resource": {"service.instance.id": "da0a6b4e-e6fd-42eb-87a8-1f033c5ceaf9", "service.name": "otelcol-contrib", "service.version": "0.128.0"}, "otelcol.component.id": "prometheus", "otelcol.component.kind": "receiver", "otelcol.signal": "metrics", "scrape_pool": "snmp", "target": "http://snmp-exporter-prometheus-snmp-exporter.helm.svc.cluster.local:9116/snmp?module=paloalto_fw&module=system&module=panorama_cpu&target=192.168.0.100", "err": "Get \"http://snmp-exporter-prometheus-snmp-exporter.helm.svc.cluster.local:9116/snmp?module=paloalto_fw&module=system&module=panorama_cpu&target=192.168.0.100\": dial tcp: lookup snmp-exporter-prometheus-snmp-exporter.helm.svc.cluster.local on 77.91.63.250:53: no such host"}


2025-06-25T12:11:23.472Z        debug   Scrape failed   {"resource": {"service.instance.id": "24d140fe-d6b6-471b-88d9-cab65d442819", "service.name": "otelcol-contrib", "service.version": "0.128.0"}, "otelcol.component.id": "prometheus", "otelcol.component.kind": "receiver", "otelcol.signal": "metrics", "scrape_pool": "snmp", "target": "http://snmp-exporter-prometheus-snmp-exporter.helm.svc.cluster.local:9116/snmp?module=paloalto_fw&module=system&module=panorama_cpu&target=192.168.0.100", "err": "Get \"http://snmp-exporter-prometheus-snmp-exporter.helm.svc.cluster.local:9116/snmp?module=paloalto_fw&module=system&module=panorama_cpu&target=192.168.0.100\": dial tcp: lookup snmp-exporter-prometheus-snmp-exporter.helm.svc.cluster.local on 77.91.63.250:53: no such host"}

2025-06-25T12:11:23.474Z        warn    internal/transaction.go:150     Failed to scrape Prometheus endpoint    {"resource": {"service.instance.id": "24d140fe-d6b6-471b-88d9-cab65d442819", "service.name": "otelcol-contrib", "service.version": "0.128.0"}, "otelcol.component.id": "prometheus", "otelcol.component.kind": "receiver", "otelcol.signal": "metrics", "scrape_timestamp": 1750853483440, "target_labels": "{__name__=\"up\", instance=\"192.168.0.100\", job=\"snmp\"}"}