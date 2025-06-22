
https://github.com/open-telemetry/opentelemetry-collector/blob/main/extension/zpagesextension/README.md
https://opentelemetry.io/docs/collector/troubleshooting/
## Co oznacza ekran „Trace Spans”?

Ekran, który pokazujesz, pochodzi z narzędzia do monitorowania aplikacji (np. OpenTelemetry, Jaeger, itp.) i przedstawia tzw. *spany* (ang. spans) w kontekście śledzenia rozproszonych żądań (distributed tracing).

### **Podstawowe pojęcia**

- **Trace (ślad)**: Reprezentuje całą podróż pojedynczego żądania przez różne usługi i komponenty systemu. To pełny obraz tego, jak żądanie przechodzi przez system od początku do końca[1][2].
- **Span (span, odcinek)**: To pojedyncza operacja lub jednostka pracy w ramach śladu. Każdy span opisuje konkretne działanie, np. obsługę żądania HTTP, zapytanie do bazy danych czy wywołanie zewnętrznego API. Spany mają relacje rodzic-dziecko, co pozwala odwzorować hierarchię i zależności między operacjami[3][1][4][5][2].

### **Co oznaczają kolumny na ekranie?**

- **Span Name**: Nazwa operacji (np. GET, exporter/enqueue), która została zarejestrowana jako span.
- **Running**: Liczba aktualnie trwających (niezakończonych) spanów dla danej operacji.
- **Latency Samples**: Próbki opóźnień – pokazują, ile razy dana operacja trwała dłużej niż określony próg czasowy (np. >10μs, >1ms, >10ms itd.). Dzięki temu możesz łatwo zidentyfikować, które operacje są wolne lub mogą być wąskim gardłem systemu[4].
- **Error Samples**: Liczba zarejestrowanych błędów dla danej operacji.

### **Jak interpretować ten ekran?**

- Pozwala on zobaczyć, które operacje są najczęściej wykonywane, jak długo trwają oraz czy występują w nich błędy.
- Analizując rozkład opóźnień (latency samples), możesz szybko wykryć, które fragmenty systemu wymagają optymalizacji lub mogą powodować problemy z wydajnością[6][4].
- Brak błędów w kolumnie Error Samples sugeruje, że operacje przebiegają poprawnie.

### **Przykład interpretacji**

- Jeśli widzisz, że operacja `exporter/kafka/metrics/metrics` ma wysoką liczbę próbek w kolumnie >100μs, oznacza to, że ta operacja często trwa powyżej 100 mikrosekund. Gdyby pojawiły się wartości w kolumnie Error Samples, wskazywałoby to na problemy z tą operacją.

### **Podsumowanie**

Ekran „Trace Spans” służy do monitorowania i analizy wydajności oraz stabilności poszczególnych operacji w systemie rozproszonym. Dzięki niemu możesz:

- Szybko wykrywać wąskie gardła i źródła opóźnień,
- Identyfikować operacje z błędami,
- Analizować zachowanie systemu na poziomie pojedynczych żądań i operacji[3][1][4][5][2].

To narzędzie jest kluczowe w nowoczesnych architekturach mikroserwisowych, gdzie żądania przechodzą przez wiele niezależnych komponentów.

[1] https://openobserve.ai/resources/trace-spans-distributed-tracing
[2] https://cloud.google.com/trace/docs/traces-and-spans
[3] https://opentelemetry.io/docs/concepts/signals/traces/
[4] https://logit.io/blog/post/traces-vs-spans/
[5] https://edgedelta.com/company/blog/what-are-spans-in-distributed-tracing
[6] https://grafana.com/docs/learning-journeys/drilldown-traces/view-details-trace-span/
[7] https://pplx-res.cloudinary.com/image/private/user_uploads/18333937/6c28c627-a74d-4a17-a49e-d34e4df39ad9/image.jpg
[8] https://docs.oracle.com/en-us/iaas/tools/typescript/2.102.2/modules/_apmtraces_lib_model_trace_span_summary_.tracespansummary.html
[9] https://www.logicmonitor.com/blog/what-are-spans-in-distributed-tracing
[10] https://signoz.io/blog/distributed-tracing-span/
[11] https://docs.wavefront.com/trace_data_details.html