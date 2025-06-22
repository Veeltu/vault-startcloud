## Potencjalne miejsca błędów w Twojej konfiguracji

Poniżej znajdziesz listę najczęstszych miejsc, gdzie mogą pojawić się błędy w przedstawionej konfiguracji OpenTelemetry Collector oraz wskazówki, na co szczególnie zwrócić uwagę podczas wdrażania tego typu rozwiązania[1][2][3].

---

### 1. Sekcja `resources`

- **Zbyt niskie lub zbyt wysokie limity** – Jeśli limity CPU/pamięci są źle dobrane do rzeczywistego obciążenia, Collector może być ubijany przez Kubernetesa (OOMKilled) lub nie wykorzystywać w pełni dostępnych zasobów[1].
- **Brak zgodności między requests a limits** – Requests powinny być zawsze mniejsze lub równe limits, co jest tutaj zachowane[1].

---

### 2. Sekcja `image`

- **Nieistniejący tag lub repozytorium** – Upewnij się, że tag `0.125.0` istnieje w podanym repozytorium i że masz do niego dostęp (autoryzacja GCP Artifact Registry)[1].
- **PullPolicy** – `IfNotPresent` może powodować użycie starego obrazu, jeśli chcesz zawsze mieć najnowszy, użyj `Always`[1].

---

### 3. Sekcja `ports`

- **Konflikt portów** – Upewnij się, że port 54526 nie jest zajęty przez inne procesy na hoście lub w klastrze[1].
- **HostPort** – Użycie `hostPort` może ograniczyć skalowalność (tylko jeden pod na węzeł dla tego portu)[1].

---

### 4. Sekcja `extraEnvs`

- **Prawidłowe przekazanie zmiennych** – Sprawdź, czy zmienne środowiskowe są poprawnie przekazywane do kontenera i czy sekrety istnieją w klastrze[2].
- **Brakujące/nieprawidłowe sekrety** – Jeśli secret `kafka-client-pwd-poc-otel-nsd-security-rw` nie istnieje lub nie zawiera klucza, Collector nie uruchomi się poprawnie[2].

---

### 5. Sekcja `receivers`

#### a) Syslog

- **Błędny adres nasłuchiwania** – `${env:MY_POD_IP}` musi być poprawnie ustawiony; jeśli nie, Collector nie będzie nasłuchiwał na właściwym interfejsie[2].
- **Port** – Musi zgadzać się z portem otwartym na kontenerze[2].

#### b) Prometheus/paloalto

- **Poprawność adresów SNMP Exportera** – Adres `snmp-exporter-prometheus-snmp-exporter.security.svc.cluster.local:9116` musi być osiągalny z podu Collector[3].
- **Poprawność OID-ów i modułów** – Moduły (`paloalto_fw`, `system`, `panorama_cpu`) muszą być zdefiniowane w SNMP Exporterze, w przeciwnym razie nie będą zbierane żadne metryki[3].
- **Parametry `auth`** – Jeśli SNMP Exporter wymaga autoryzacji, a `auth: []` jest puste, może nie działać z niektórymi urządzeniami[3].
- **Zakomentowane adresy** – Jeśli chcesz monitorować więcej urządzeń, musisz je odkomentować i upewnić się, że są dostępne w sieci[3].

---

### 6. Sekcja `exporters`

- **Dostępność brokera Kafka** – Adres `otel-kafka-bootstrap.kafka-cluster:9094` musi być osiągalny z podu Collector[2].
- **Poprawność danych logowania** – Zmienna środowiskowa i sekret muszą być poprawne[2].
- **Zgodność wersji protokołu** – `protocol_version: 3.9.0` musi być wspierany przez Twój broker Kafka[2].

---

### 7. Sekcja `service.pipelines`

- **Zgodność nazw receiverów i eksporterów** – Wszystkie wpisy w pipelines (`otlp`, `prometheus`, `prometheus/paloalto`, `debug`, `kafka/metrics`) muszą być zdefiniowane w sekcjach `receivers` i `exporters`[1].
- **Brak trace pipeline** – Jeśli planujesz zbierać ślady (traces), musisz dodać odpowiedni pipeline[1].
- **Brak eksportu logów do Kafki** – Logi są eksportowane tylko do debug, jeśli chcesz mieć je w Kafce, musisz dodać odpowiedniego eksportera[1].

---

### 8. Inne typowe błędy

- **Błędy formatowania YAML** – Wcięcia, nieprawidłowe typy danych (np. liczby bez cudzysłowów przy wymaganym stringu)[1].
- **Brak wymaganych sekcji** – Np. brak sekcji `processors` lub `exporters` w pipeline powoduje błąd startu Collectora[1].
- **Nieaktualna dokumentacja** – Upewnij się, że używasz wersji konfiguracji zgodnej z wersją obrazu Collectora[1].

---

## Podsumowanie

Najczęstsze błędy to: nieistniejące sekrety, nieosiągalne adresy sieciowe, brakujące lub błędne moduły SNMP, błędy w pipeline (niedopasowanie nazw), błędy formatowania YAML oraz niezgodność wersji komponentów. Zalecane jest testowanie konfiguracji etapami i sprawdzanie logów Collectora po wdrożeniu[1][2][3]. 

---

[1]: https://opentelemetry.io/docs/collector/configuration/
[2]: https://github.com/open-telemetry/opentelemetry-collector/blob/main/docs/troubleshooting.md
[3]: https://github.com/prometheus/snmp_exporter/blob/main/example.yml

[1] tools.observability_pipeline
[2] tools.metrics_collection
[3] tools.snmp_monitoring


## Czy `replacement: snmp-exporter-prometheus-snmp-exporter.security.svc.cluster.local:9116` jest poprawne?

### Co robi ten wpis?

Wartość `replacement: snmp-exporter-prometheus-snmp-exporter.security.svc.cluster.local:9116` w sekcji `relabel_configs` przekierowuje wszystkie zapytania SNMP z Prometheusa do usługi SNMP Exportera działającej w Twoim klastrze Kubernetes, na porcie 9116[#][#].

---

### Kiedy to jest poprawne?

- **Poprawne:**  
  Jeśli usługa SNMP Exportera istnieje w namespace `security` pod nazwą `snmp-exporter-prometheus-snmp-exporter` i nasłuchuje na porcie 9116 (czyli masz w klastrze Service typu ClusterIP lub Headless o tej nazwie), to wpis jest prawidłowy[#].
- **Niepoprawne:**  
  Jeśli:
  - Nazwa usługi jest inna,
  - Namespace jest inny,
  - Port jest inny niż 9116,
  - Usługa nie została wdrożona lub nie jest osiągalna z podu Collectora,
  
  wtedy Prometheus nie będzie w stanie pobierać metryk SNMP i pojawią się błędy połączenia[#].

---

### Jak sprawdzić poprawność?

1. **Sprawdź nazwę i namespace usługi:**
   - Polecenie:  
     `kubectl get svc -n security | grep snmp-exporter`
   - Upewnij się, że istnieje dokładnie taka usługa.

2. **Sprawdź port:**
   - Polecenie:  
     `kubectl describe svc snmp-exporter-prometheus-snmp-exporter -n security`
   - Sprawdź, czy port 9116 jest wystawiony.

3. **Sprawdź dostępność z podu Collectora:**
   - Możesz wykonać test połączenia z wnętrza podu Collectora, np. używając `curl` lub `nc` na adres i port.

---

### Najczęstsze błędy

- Literówki w nazwie usługi lub namespace.
- Usługa SNMP Exportera nie została wdrożona lub jest w innym namespace.
- Port inny niż 9116.
- Brak uprawnień sieciowych (np. NetworkPolicy blokująca ruch).

---

### Podsumowanie

Wartość `replacement: snmp-exporter-prometheus-snmp-exporter.security.svc.cluster.local:9116` jest poprawna **tylko wtedy**, gdy dokładnie taka usługa istnieje w Twoim klastrze i nasłuchuje na tym porcie. Jeśli masz wątpliwości, sprawdź nazwę, namespace i port usługi w Kubernetes[#][#].

---

[#] https://github.com/prometheus/snmp_exporter/blob/main/example.yml  
[#] https://prometheus.io/docs/guides/snmp/  
[#] https://opentelemetry.io/docs/collector/configuration/#prometheusreceiver