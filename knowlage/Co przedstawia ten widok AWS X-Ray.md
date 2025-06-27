## Co przedstawia ten widok AWS X-Ray?

![[Pasted image 20250626120251.png]]


To jest zrzut ekranu z konsoli AWS X-Ray, przedstawiający szczegółową oś czasu (Segments Timeline) dla jednego żądania HTTP przechodzącego przez Twój system oparty o API Gateway i AWS Lambda[1][2][3].

### Kluczowe pojęcia

- **Trace (ślad)** – zbiór wszystkich segmentów wygenerowanych przez pojedyncze żądanie w Twojej aplikacji. Pozwala prześledzić całą ścieżkę żądania przez różne komponenty[2][3].
- **Segment** – reprezentuje jedną usługę lub komponent obsługujący żądanie, np. API Gateway, Lambda, DynamoDB. Segmenty mogą mieć podsegmenty (subsegments), które pokazują szczegóły pracy danego komponentu[2][4][3].
- **Subsegment** – szczegółowy podział pracy w ramach segmentu, np. wywołania HTTP, operacje na bazie danych[4][5].

### Szczegółowy opis widoku

#### 1. Główna oś czasu (Segments Timeline)
Pokazuje, ile czasu zajęło przetwarzanie żądania przez każdy komponent oraz jak te czasy się na siebie nakładają.

#### 2. Segmenty

- **x-tray-test/dev AWS::ApiGateway::Stage**
  - To segment reprezentujący API Gateway (stage `dev` w API `x-tray-test`).
  - Widać, że żądanie GET na endpoint `/dev/dwa` zostało obsłużone w 21 ms (status 200).
  - Podsegment Lambda pokazuje, że wywołano funkcję Lambda, która obsłużyła żądanie w 18 ms (status 200).

- **my-return-event-context AWS::Lambda**
  - Segment Lambda, który pokazuje obsługę żądania przez funkcję Lambda.
  - Czas trwania 9 ms (status 200).

- **my-return-event-context AWS::Lambda::Function**
  - Szczegółowy segment funkcji Lambda.
  - Czas trwania 23 ms (brak kodu odpowiedzi, bo to wewnętrzny pomiar).
  - Widać również subsegment "Overhead" – czas narzutu środowiska Lambda (np. inicjalizacja środowiska, cold start).

#### 3. Oś czasu (po prawej)
- Pokazuje, kiedy dokładnie zaczęły i skończyły się poszczególne segmenty względem siebie (w milisekundach).
- Pozwala zidentyfikować, które elementy systemu są wąskim gardłem lub generują największe opóźnienia.

#### 4. Szczegóły żądania
- Widać dokładny URL żądania oraz operacje wykonywane przez Lambda.
- Można zobaczyć, które wywołania trwały najdłużej, a które były natychmiastowe.

### Co daje ten widok?

- **Diagnostyka wydajności** – Możesz zobaczyć, gdzie pojawiają się opóźnienia w obsłudze żądania (np. czy najwięcej czasu zajmuje API Gateway, Lambda, czy inne komponenty)[6][3].
- **Analiza błędów** – Statusy odpowiedzi i ewentualne błędy są widoczne bezpośrednio na osi czasu.
- **Mapowanie architektury** – Widzisz, jak żądanie przepływa przez różne warstwy Twojej aplikacji w chmurze AWS.

### Podsumowanie

Ten widok X-Ray pokazuje szczegółowy przebieg pojedynczego żądania HTTP przez Twój system: od wejścia do API Gateway, przez wywołanie funkcji Lambda, aż po zakończenie obsługi. Dzięki temu możesz łatwo analizować wydajność, identyfikować wąskie gardła i rozwiązywać problemy z opóźnieniami lub błędami w architekturze serverless[2][6][3].

[1] https://pplx-res.cloudinary.com/image/private/user_uploads/18333937/348ce25c-2cb8-4a51-ae95-5324aaf02991/image.jpg
[2] https://docs.aws.amazon.com/xray/latest/devguide/xray-concepts.html
[3] https://www.trek10.com/blog/aws-xray-xplained
[4] https://docs.aws.amazon.com/xray/latest/devguide/xray-api-segmentdocuments.html
[5] https://docs.amazonaws.cn/en_us/xray/latest/devguide/xray-api-segmentdocuments.html
[6] https://docs.aws.amazon.com/xray/latest/devguide/aws-xray.html
[7] https://newsletter.simpleaws.dev/p/observability-with-aws-x-ray
[8] https://docs.aws.amazon.com/xray/latest/api/API_TraceSummary.html
[9] https://www.youtube.com/watch?v=rmxabNtu9C4
[10] https://aws.plainenglish.io/aws-x-ray-a-comprehensive-guide-for-distributed-applications-8c6de6b2eca4?gi=092a7c58eee3
[11] https://docs.aws.amazon.com/xray/latest/devguide/xray-api-sendingdata.html
