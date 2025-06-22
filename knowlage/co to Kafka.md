

# Kafka
#kafka

### Co to jest Kafka?

**Kafka** (pełna nazwa: **Apache Kafka**) to otwartoźródłowa platforma programistyczna służąca do przetwarzania, przesyłania i przechowywania strumieni danych w czasie rzeczywistym. Została stworzona przez firmę LinkedIn, a obecnie jest rozwijana przez Apache Software Foundation.

#### Do czego służy Kafka?

Kafka jest wykorzystywana wszędzie tam, gdzie trzeba:
- **Szybko i niezawodnie przesyłać duże ilości danych** pomiędzy różnymi systemami lub aplikacjami,
- **Przetwarzać dane w czasie rzeczywistym** (np. logi, zdarzenia, dane telemetryczne, kliknięcia użytkowników),
- **Budować systemy mikroserwisowe** i architektury oparte na zdarzeniach (event-driven architecture),
- **Agregować i analizować dane** z wielu źródeł.

#### Jak działa Kafka? (w skrócie)

- **Producent (Producer)** – wysyła dane (wiadomości) do Kafki.
- **Broker** – serwer Kafki, który przechowuje i zarządza przesyłanymi danymi.
- **Temat (Topic)** – logiczna „kolejka”, do której trafiają wiadomości.
- **Konsument (Consumer)** – odbiera wiadomości z wybranego tematu.

Kafka jest bardzo wydajna, skalowalna i odporna na awarie. Dane mogą być przechowywane przez określony czas, a wielu konsumentów może je czytać niezależnie.

#### Przykłady zastosowań

- Analiza logów i zdarzeń w czasie rzeczywistym (np. bezpieczeństwo, monitoring)
- Integracja systemów w dużych firmach
- Przetwarzanie danych z urządzeń IoT
- Systemy rekomendacji i analityka zachowań użytkowników

#### Ciekawostka

Nazwa „Kafka” pochodzi od nazwiska pisarza Franza Kafki, co miało być żartobliwym odniesieniem do „skomplikowanych procesów przetwarzania wiadomości”.

---

**Podsumowując:**  
Kafka to platforma do przesyłania i przetwarzania dużych ilości danych w czasie rzeczywistym – szybka, skalowalna i szeroko wykorzystywana w nowoczesnych systemach IT.