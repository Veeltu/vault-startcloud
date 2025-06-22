Oto lista wszystkich komend z Twojego zestawu, z **dodanym prefiksem `microk8s` do wszystkich poleceń kubectl** oraz z drobnymi poprawkami (np. poprawne użycie zmiennych w logach). Polecenia są poprawne i gotowe do użycia z MicroK8s.

---

## 1. Sprawdzenie stanu Terraform/Terragrunt

```bash
terragrunt state list
terragrunt state show helm_release.traefik
```
*(Te polecenia nie wymagają microk8s)*

---

## 2. Sprawdzenie przestrzeni nazw

```bash
microk8s kubectl get namespaces
microk8s kubectl describe namespace traefik
microk8s kubectl describe namespace network
microk8s kubectl describe namespace otel-collector
```

---

## 3. Sprawdzenie Traefik

```bash
microk8s kubectl get pods -n traefik
microk8s kubectl describe pods -n traefik
microk8s kubectl get svc -n traefik
microk8s kubectl logs -n traefik $(microk8s kubectl get pods -n traefik -o name | head -1)
microk8s kubectl get configmap -n traefik
```

---

## 4. Sprawdzenie Gateway API

```bash
microk8s kubectl get gateway -n network
microk8s kubectl describe gateway otel -n network
microk8s kubectl get tcproute -n network
microk8s kubectl describe tcproute syslogtcp -n network
microk8s kubectl get tlsroute -n network
microk8s kubectl describe tlsroute syslogtcptls -n network
```

---

## 5. Sprawdzenie certyfikatów

```bash
microk8s kubectl get certificates -n traefik
microk8s kubectl describe certificate gw-observability-test-pndrs-de -n traefik
microk8s kubectl get secrets -n traefik | grep gw-observability-test-pndrs-de
```

---

## 6. Sprawdzenie OpenTelemetry Collector

```bash
microk8s kubectl get opentelemetrycollector -n otel-collector
microk8s kubectl get pods -n otel-collector
microk8s kubectl logs -n otel-collector $(microk8s kubectl get pods -n otel-collector -o name | head -1)
```

---

## 7. Sprawdzenie usług testowych

```bash
microk8s kubectl get svc test-mock -n network
microk8s kubectl describe svc test-mock -n network
```

---

## 8. Sprawdzenie połączeń sieciowych

```bash
microk8s kubectl get svc -n traefik -o json | jq '.items[].spec.ports'
microk8s kubectl get endpoints -n traefik
microk8s kubectl get endpoints -n network
```

---

## 9. Sprawdzenie dostępności zewnętrznej

```bash
microk8s kubectl get svc -n traefik -o jsonpath='{.items[0].status.loadBalancer.ingress[0].ip}'
curl -v http://:8000
curl -vk https://:8443
```
*(Pierwsze polecenie z microk8s, curl bez microk8s)*

---

## 10. Sprawdzenie logów i zdarzeń

```bash
microk8s kubectl get events -n traefik
microk8s kubectl get events -n network
microk8s kubectl get events --sort-by='.metadata.creationTimestamp'
```

---

**Wszystkie komendy są poprawne i dostosowane do MicroK8s.**  
W poleceniach z logami oraz pobieraniem nazw podów, użyto `microk8s kubectl` również w poleceniach wewnątrz, by uniknąć błędów w środowiskach bez aliasu `kubectl`.