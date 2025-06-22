#grafana-name
${__field.labels["exported_target"]}


```
sudo microk8s kubectl delete pod -l app=prometheus -n network

```

#diagnostic-otel

``` diagnostics
sudo microk8s kubectl get ingress -n network
sudo microk8s kubectl describe ingress prometheus-ui -n network
sudo microk8s kubectl get svc -n network
sudo microk8s kubectl get pods -n network
sudo microk8s kubectl get configmap prometheus-config -n network -o yaml
sudo microk8s kubectl get configmap otel-collector-config -n network -o yaml
```

```
curl http://localhost:9090/api/v1/targets | jq

sudo microk8s kubectl get configmap prometheus-config -n network -o yaml

sudo microk8s kubectl get configmap otel-collector-config -n network -o yaml

sudo microk8s kubectl delete pod -l app=prometheus -n network

database-prometheus for grafana
http://prometheus.monitoring.svc.cluster.local:9090


```


```
sudo microk8s kubectl get pods -A                                # Wyświetl wszystkie pody we wszystkich namespace'ach
sudo microk8s kubectl get svc -A                                 # Wyświetl wszystkie serwisy we wszystkich namespace'ach
sudo microk8s kubectl get ingress -A                             # Wyświetl wszystkie ingressy we wszystkich namespace'ach

sudo microk8s kubectl get ns                                     # Pokaz wszystkie namespaces
sudo microk8s kubectl get pods -n network                        # Pokaż pody w namespace 'network' (np. OTel Collector)
sudo microk8s kubectl get pods -n monitoring                     # Pokaż pody w namespace 'monitoring' (np. Prometheus)


sudo microk8s kubectl logs deployment/otel-collector -n network  # Pokaż logi z deploymentu OTel Collector
sudo microk8s kubectl logs deployment/prometheus -n monitoring   # Pokaż logi z deploymentu Prometheus

sudo microk8s kubectl get configmap otel-collector-config -n network -o yaml   # Pokaż konfigurację OTel Collector
sudo microk8s kubectl get configmap prometheus-config -n monitoring -o yaml    # Pokaż konfigurację Prometheusa

sudo microk8s kubectl describe ingress otel-collector-metrics -n network       # Szczegóły i eventy Ingressu Collectora
sudo microk8s kubectl describe ingress prometheus-ingress -n monitoring        # Szczegóły i eventy Ingressu Prometheusa

sudo microk8s kubectl port-forward svc/prometheus 9090:9090 -n monitoring      # Udostępnij Prometheusa lokalnie na porcie 9090
sudo microk8s kubectl port-forward svc/otel-collector 8889:8889 -n network     # Udostępnij OTel Collector lokalnie na porcie 8889

curl http://localhost:9090/api/v1/targets | jq                    # Pokaż status targetów scrape w Prometheusie (wymaga jq)
curl http://localhost:8889/metrics | head                         # Pokaż początek metryk eksportowanych przez OTel Collector

sudo microk8s kubectl exec -it deployment/otel-collector -n network -- wget -qO- http://192.168.0.100:9481/metrics | head   # Sprawdź czy Collector widzi node-exporter poza klastrem

sudo microk8s kubectl delete pod -l app=prometheus -n monitoring  # Zrestartuj Prometheusa (np. po zmianie configmapy)
sudo microk8s kubectl delete pod -l app=otel-collector -n network # Zrestartuj OTel Collector (np. po zmianie configmapy)

sudo microk8s kubectl edit ingress prometheus-ingress -n monitoring # Edytuj Ingress Prometheusa "na żywo"
sudo microk8s kubectl edit ingress otel-collector-metrics -n network # Edytuj Ingress Collectora "na żywo"

```
