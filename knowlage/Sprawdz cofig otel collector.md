Najlepszy i najbardziej wiarygodny sposób, by **sprawdzić, czy cały config OpenTelemetry Collector został zaaplikowany i wszystkie zasoby (receivery, procesory, eksportery, pipeline’y) faktycznie działają**, to:

---

## 1. **Sprawdź logi poda Collectora**

**To najważniejsze źródło prawdy.**  
Collector podczas startu wypisuje w logach, które komponenty zostały załadowane i uruchomione.

```bash
microk8s kubectl logs -n network 
```

W logach szukaj wpisów typu:
- `Starting receiver...` lub `Receiver started...` (np. `Receiver k8s_cluster started`)
- `Pipeline started...`
- Ewentualnych błędów (np. `error loading config`, `failed to start receiver`)

**Jeśli w logach nie ma błędów, a są wpisy o starcie wszystkich receiverów, procesorów i eksporterów z Twojego configu, to znaczy, że Collector poprawnie załadował całą konfigurację i uruchomił wszystkie zasoby**[2][4].

---

## 2. **Sprawdź, czy ConfigMap zawiera Twój config**

Upewnij się, że w Kubernetesie jest aktywna najnowsza wersja configu:

```bash
microk8s kubectl get configmap otel-collector-config -n network -o yaml
```
W sekcji `data` powinieneś zobaczyć dokładnie ten config, który chcesz mieć zaaplikowany (np. sekcje `k8s_cluster`, `syslog/rfc5424`, `prometheus/vyos` itd.)[1][2].

---

## 3. **Sprawdź, czy porty są otwarte w podzie**

Wejdź do poda i sprawdź nasłuchujące porty:

```bash
microk8s kubectl exec -n network  -- netstat -tuln
```
Powinny być widoczne porty, które zadeklarowałeś w configu (np. 4317, 4318, 54526, 54527, 54528)[1].

---

## 4. **Sprawdź, czy Collector działa funkcjonalnie**

- **Testuj wysyłkę danych** do każdego receivera (np. OTLP, syslog, Prometheus).
- Sprawdź, czy dane pojawiają się w docelowym eksporcie (np. w Kafka, Prometheus, debug logach).

---

## 5. **Podsumowanie – Checklista**

| Krok                | Co sprawdzić?                                   | Oczekiwany efekt                                  |
|---------------------|------------------------------------------------|---------------------------------------------------|
| Logi poda           | Start receiverów, brak błędów                   | `Receiver ... started`, brak errorów              |
| ConfigMap           | Zawiera Twój aktualny config                    | Wszystkie sekcje obecne                           |
| Porty w podzie      | Nasłuch na portach z configu                    | Porty otwarte (np. 4317, 54526 itd.)              |
| Funkcjonalność      | Testowa wysyłka danych do receiverów            | Dane docierają do eksporterów                     |

---

**W skrócie:**
- Najlepszym i najpewniejszym sposobem jest analiza logów poda Collectora – tylko one pokażą, czy Collector poprawnie załadował i uruchomił wszystkie zasoby z configu oraz czy nie było błędów podczas startu[2][4][5].
- Sprawdzenie ConfigMap i portów to dodatkowa weryfikacja, ale tylko logi dadzą Ci 100% pewności, że Collector działa zgodnie z configiem.

---

Jeśli chcesz, możesz wkleić fragment logów startowych Collectora – pomogę je zinterpretować!

[1] https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/18333937/d59128cb-dc0a-4b65-aed1-7ae108f41bf7/paste.txt
[2] https://github.com/open-telemetry/opentelemetry-collector-contrib/blob/main/exporter/datadogexporter/examples/k8s-chart/configmap.yaml
[3] https://last9.io/blog/opentelemetry-filelog-receiver-kubernetes-log-collection/
[4] https://www.groundcover.com/opentelemetry/opentelemetry-logs
[5] https://www.ibm.com/docs/en/instana-observability/current?topic=opentelemetry-collecting-kubernetes-red-hat-openshift-logs
[6] https://grafana.com/docs/opentelemetry/collector/send-logs-to-loki/kubernetes-logs/
[7] https://docs.chronosphere.io/ingest/metrics-traces/otel/otel-ingest
[8] https://github.com/SigNoz/otel-collector-k8s/blob/main/agent/infra-metrics.yaml
[9] https://docs.splunk.com/observability/gdi/opentelemetry/collector-kubernetes/install-k8s-manifests.html
[10] https://stackoverflow.com/questions/64113850/how-to-get-environment-variable-in-configmap
[11] https://docs.datadoghq.com/opentelemetry/setup/collector_exporter/deploy/
[12] https://www.youtube.com/watch?v=3Eo91pGeI54
[13] https://grafana.com/docs/opentelemetry/collector/opentelemetry-collector/kubernetes-logs/
[14] https://opentelemetry.io/docs/kubernetes/getting-started/
[15] https://grafana.com/docs/grafana-cloud/monitor-infrastructure/kubernetes-monitoring/configuration/helm-chart-config/otel-collector/
[16] https://grafana.com/docs/grafana-cloud/monitor-applications/application-observability/collector/opentelemetry-collector-kubernetes-stdout/
[17] https://betterstack.com/community/guides/observability/opentelemetry-collector-kubernetes-helm/
[18] https://last9.io/blog/opentelemetry-collector-with-docker/
[19] https://www.youtube.com/watch?v=BHTBTOgd5WU
[20] https://github.com/open-telemetry/opentelemetry-collector-contrib/issues/30928