
#### questions:
- co to otel
- co to prometheus
- skoro prometh pobiera tylko metryki, to co pobeira logi i traces 
- w jaki sposob sa przydzielane do kafka-topic konkretne metryki
- co to jobname-prometh - label for targets

### PROMETHEUS
open-source systems monitoring and alerting toolkit

Prometheus collects and stores its metrics as time series data, i.e. metrics information is stored with the timestamp at which it was recorded, alongside optional key-value pairs called labels.

metric types- counter,summaries and histograms, gauge,
glownie pull base
- server
- metrics storage database
- UI
- api
- alerting
- query language - promQL



### OTEL
OTEL - framework służący do zbierania, przetwarzania oraz eksportowania danych telemetrycznych (logs, metrics i traces) w środowiskach rozproszonych i chmurowych. 

Komponents:
-recivers (pull,push(scrape data))
https://github.com/open-telemetry/opentelemetry-collector-contrib/tree/main/receiver/snmpreceiver
-processors
https://github.com/open-telemetry/opentelemetry-collector-contrib/tree/main/processor
-exporters
https://github.com/open-telemetry/opentelemetry-collector-contrib/blob/main/exporter/kafkaexporter/README.md

-connectors
-extensions
-customs many more
and pipelices topic

### Telemetry producer - VyOS

-processors in config:
batch: Groups telemetry data into larger batches to optimize sending to exporters.

memory_limiter: Monitors and limits the Collector’s memory usage to prevent running out of memory (limit_mib: 1500, spike_limit_mib: 512, check_interval: 5s).

k8sattributes: Enriches telemetry data with Kubernetes metadata (like pod name, namespace, deployment, node), using service account authorization.

pod_association: Defines the order in which the Collector tries to associate telemetry data with a specific Kubernetes pod (first by IP, then by UID, finally by connection).
***

![[Pasted image 20250609220123.png]]


https://last9.io/blog/opentelemetry-with-grafana/



otel-daemonset to nic innego jak instancja OpenTelemetry Collector uruchomiona jako Kubernetes DaemonSet. Oznacza to, że na każdym węźle klastra Kubernetes działa osobna kopia tego kolektora


- poprawic rysunekj
- gateways
- i poprawic ten tstan z errorami