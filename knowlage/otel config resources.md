**OTLP** (OpenTelemetry Protocol) to otwarty, standaryzowany protokół komunikacyjny używany do przesyłania danych telemetrycznych – takich jak metryki, logi i trasy (traces) – pomiędzy aplikacjami, agentami, kolektorami i backendami w ekosystemie [OpenTelemetry](https://opentelemetry.io/).

## Najważniejsze informacje o OTLP

- **Cel:** Umożliwia jednolity sposób przesyłania danych telemetrycznych (metrics, traces, logs) z różnych źródeł do systemów monitoringu i analizy.
- **Format:** Obsługuje komunikację zarówno przez gRPC (protokół binarny, szybki), jak i HTTP/JSON (bardziej czytelny dla ludzi).
- **Zastosowanie:** Najczęściej wykorzystywany do przesyłania danych z aplikacji do OpenTelemetry Collector, a następnie do backendów takich jak Prometheus, Jaeger, Grafana, Datadog itp.
- **Uniwersalność:** Zastępuje starsze, specyficzne protokoły (np. Jaeger, Zipkin) jednym, uniwersalnym standardem.

## Jak działa OTLP?

1. **Aplikacja** generuje dane telemetryczne (np. trace, metryki).
2. **Agent/SDK** (np. OpenTelemetry SDK) wysyła te dane przez OTLP do **Collector’a**.
3. **Collector** odbiera dane, może je przetwarzać, agregować i eksportować do różnych backendów.

## Przykład konfiguracji OTLP w OpenTelemetry Collector

```yaml
receivers:
  otlp:
    protocols:
      grpc:
        endpoint: 0.0.0.0:4317
      http:
        endpoint: 0.0.0.0:4318
```

- **4317** – domyślny port dla OTLP/gRPC
- **4318** – domyślny port dla OTLP/HTTP

## Podsumowanie

**OTLP** to nowoczesny, uniwersalny protokół do przesyłania danych telemetrycznych w środowiskach monitoringu i obserwowalności. Ułatwia integrację różnych narzędzi i standaryzuje sposób wymiany danych.

***
***

