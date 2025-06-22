# diagnostyka:

Aby sprawdzić, co aktualnie widzi i zbiera Twój Prometheus w Kubernetes (MicroK8s), wykonaj te polecenia w terminalu:

---

### 1. Sprawdź status podów Prometheusa
```bash
sudo microk8s kubectl get pods -n monitoring
```

---

### 2. Sprawdź logi Prometheusa (czy nie ma błędów scrape/config)
```bash
sudo microk8s kubectl logs deployment/prometheus -n monitoring
```

---

### 3. Sprawdź konfigurację Prometheusa (czy target jest poprawny)
```bash
sudo microk8s kubectl get configmap prometheus-config -n monitoring -o yaml
```
W sekcji `prometheus.yml` powinieneś mieć:
```yaml
scrape_configs:
  - job_name: 'otel-collector'
    metrics_path: /metrics
    static_configs:
      - targets:
        - otel-collector.network.svc.cluster.local:8889
```

---

### 4. Sprawdź status targetów przez web UI lub API

**a) Port-forward do Prometheusa:**
```bash
sudo microk8s kubectl port-forward svc/prometheus 9090:9090 -n monitoring
```

**b) W przeglądarce:**  
Przejdź do: [http://localhost:9090/targets](http://localhost:9090/targets)  
Sprawdź, czy job `otel-collector` jest **UP**.

**c) Przez API (w terminalu):**
```bash
curl http://localhost:9090/api/v1/targets | jq
```
Szukaj `"health": "up"` oraz `"lastError": null` dla targetu `otel-collector`.

---

### 5. Sprawdź, czy Prometheus widzi metryki z Collectora

Z podu Prometheusa:
```bash
sudo microk8s kubectl exec -it deployment/prometheus -n monitoring -- curl -s http://otel-collector.network.svc.cluster.local:8889/metrics | head
```
Powinieneś zobaczyć początek eksportowanych metryk.

---

**Podsumowanie:**  
- Twój scrape_config jest już poprawny (używasz DNS Service Kubernetes, nie Ingress).
- Jeśli target jest **UP** i widzisz metryki, wszystko działa.
- Jeśli nadal są błędy, sprawdź logi Prometheusa i Collectora oraz wynik `curl` z podu Prometheusa na adres Collectora.

---

**Jeśli któryś etap nie działa, wrzuć wynik polecenia tutaj – pomogę znaleźć przyczynę!**