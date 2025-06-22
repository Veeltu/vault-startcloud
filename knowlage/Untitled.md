Oto rozwiązanie problemu z port-forward i błędem połączenia:

---

## 🔍 Przyczyna błędu
Błąd `connection refused` występuje, ponieważ **OpenTelemetry Collector w podzie nie nasłuchuje na `localhost` (127.0.0.1)**, tylko na adresie IP poda (np. `192.168.1.5`). Gdy wykonujesz `port-forward`, Kubernetes próbuje połączyć się z `localhost` wewnątrz poda, ale Collector nie jest tam dostępny.

---

## 🛠️ Jak to naprawić?

### 1. Zmień konfigurację OTLP w Collectorze
W pliku `otel-collector-config.yaml` zmień endpointy OTLP na `0.0.0.0`, aby Collector nasłuchiwał na wszystkich interfejsach:
```yaml
receivers:
  otlp:
    protocols:
      grpc:
        endpoint: 0.0.0.0:4317  # Zmiana z ${env:MY_POD_IP}
      http:
        endpoint: 0.0.0.0:4318   # Zmiana z ${env:MY_POD_IP}
```

### 2. Upewnij się, że wersja obrazu jest stabilna
W Deploymentcie zmień tag obrazu na konkretną wersję (np. `0.97.0`):
```terraform
image = "otel/opentelemetry-collector-contrib:0.97.0"  # Unikaj "latest"
```

### 3. Przeładuj konfigurację
Usuń istniejącego poda, aby Kubernetes stworzył go od nowa z poprawioną konfiguracją:
```bash
kubectl delete pod -n network -l app=otel-collector
```

### 4. Sprawdź logi poda
Upewnij się, że Collector startuje bez błędów:
```bash
kubectl logs -n network  -f
```
Szukaj komunikatów:
> Listening on [::]:4317 (gRPC)  
> Listening on [::]:4318 (HTTP)

---

## ✅ Jak przetestować?

### Krok 1: Uruchom port-forward
```bash
microk8s kubectl port-forward -n network svc/otel-collector 4317:4317
```

### Krok 2: Wyślij testowe dane
Użyj narzędzia `telemetrygen`:
```bash
telemetrygen traces --otlp-insecure --otlp-endpoint=localhost:4317
```

Jeśli wszystko działa, w logach Collectora pojawią się informacje o odebranych danych:
> Traces received: 3

---

## 📋 Podsumowanie zmian
| Plik/zasób          | Zmiana                                                                 |
|---------------------|-----------------------------------------------------------------------|
| `config.yaml`       | `endpoint: 0.0.0.0:4317` zamiast `${env:MY_POD_IP}`                   |
| Deployment (TF)     | Ustawiono wersję obrazu (np. `0.97.0`)                                |
| Service (TF)        | Poprawiono nazwy portów (opcjonalnie dla spójności)                   |

Po tych zmianach port-forward powinien działać bez błędów, a Collector będzie dostępny na `localhost:4317`.