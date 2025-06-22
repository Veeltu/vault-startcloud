

## **1. Traefik – co robi?**

- **Traefik to reverse proxy i load balancer** – kieruje ruch sieciowy (HTTP, HTTPS, TCP) do odpowiednich usług w klastrze Kubernetes.
- **Obsługuje routing na podstawie reguł Gateway API** – automatycznie wykrywa nowe usługi i konfiguruje routing.
- **Zapewnia terminację TLS (HTTPS)** – dzięki certyfikatom z cert-manager możesz zabezpieczyć ruch do swoich usług.
- **Umożliwia logowanie dostępu** – logi są zapisywane w formacie JSON, co ułatwia integrację z systemami monitoringu.

---

## **2. Certyfikaty – do czego służą?**

- **Certyfikaty są używane do szyfrowania ruchu HTTPS** – czyli zabezpieczają połączenia między użytkownikiem a usługami (np. Grafana, dashboardy, API).
- **Certyfikaty są automatycznie odnawiane** przez cert-manager – nie musisz martwić się o wygasające certyfikaty.
- **Certyfikaty są przypisywane do konkretnych domen** (np. `gw.observability.test.pndrs.de`) – dzięki temu możesz mieć wiele zabezpieczonych usług pod różnymi adresami.

---

## **3. Do czego to wszystko razem służy?**

- **Udostępniasz usługi (np. Grafana, Prometheus, własne API) na zewnątrz klastra** – użytkownicy mogą się łączyć przez HTTPS, a ruch jest bezpieczny.
- **Masz automatyczny routing i load balancing** – Traefik sam wykrywa nowe usługi i konfiguruje routing.
- **Masz monitoring i logowanie** – Traefik wystawia metryki Prometheus (np. liczba requestów, czas odpowiedzi), które możesz zbierać i wizualizować w Grafanie[1][5].
- **Masz automatyczne zarządzanie certyfikatami** – certyfikaty są odnawiane bez Twojej ingerencji.

---

## **4. Przykład praktycznego zastosowania**

- **Użytkownik wpisuje w przeglądarce:**
  ```
  https://gw.observability.test.pndrs.de
  ```
- **Traefik odbiera żądanie, sprawdza certyfikat i kieruje ruch do odpowiedniej usługi (np. Grafana).**
- **Wszystko jest szyfrowane, a ruch jest monitorowany i logowany.**

---

## **Podsumowanie**

**Traefik + certyfikaty służą do bezpiecznego udostępniania usług w klastrze Kubernetes, automatycznego routingu, load balancingu i monitorowania ruchu.**  
Dzięki temu możesz łatwo i bezpiecznie udostępniać aplikacje użytkownikom lub innym systemom, a całość jest monitorowana i zarządzana automatycznie[1][5].

[1] https://grafana.com/docs/grafana-cloud/monitor-infrastructure/integrations/integration-reference/integration-traefik/
[2] https://doc.traefik.io/traefik-hub/api-gateway/reference/install/observability/ref-metrics
[3] https://evoila.com/blog/building-open-source-observability-stack-opentelemetry/
[4] https://opentelemetry.io/blog/2023/k8s-runtime-observability/
[5] https://brianchristner.io/deploy-configure-monitor-traefik-prometheus-grafana/
[6] https://doc.traefik.io/traefik/observability/metrics/overview/
[7] https://grafana.com/docs/grafana-cloud/monitor-infrastructure/kubernetes-monitoring/configuration/config-other-methods/prometheus/prometheus-operator/
[8] https://prometheus.io/docs/instrumenting/exporters/
[9] https://grafana.com/docs/grafana-cloud/monitor-applications/asserts/enable-prom-metrics-collection/messaging-frameworks/kafka/
[10] https://github.com/traefik/traefik/issues/11272