
```
#
netstat -tulnp

#dostepny lokalnie ?
nc -vz localhost 4317
```

---

## Co pokazuje polecenie `netstat -tulnp`?

To polecenie pokazuje wszystkie **nasłuchujące porty** na Twoim komputerze – czyli na jakich adresach i portach różne programy (serwery, usługi) czekają na połączenia z sieci.

- **tcp**/**udp** – protokół (TCP lub UDP)
- **Local Address** – adres IP i port na którym serwis nasłuchuje (np. `127.0.0.1:8889`)
- **State** – stan połączenia (np. LISTEN – nasłuchuje)
- **PID/Program name** – numer procesu i nazwa programu (jeśli masz uprawnienia root)

---

## Co oznaczają te wpisy?

Przykład:
```
tcp        0      0 127.0.0.1:8889          0.0.0.0:*               LISTEN      -
```
- **tcp** – protokół TCP
- **127.0.0.1:8889** – program nasłuchuje na porcie 8889, ale tylko na lokalnym interfejsie (localhost)
- **LISTEN** – czeka na połączenia
- **-** – nie widać nazwy programu (brak uprawnień root)

---

## Najważniejsze rzeczy dla Ciebie

- **127.0.0.1:8889** (oraz **::1:8889** dla IPv6) – oznacza, że jakiś program (prawdopodobnie Twój OpenTelemetry Collector) nasłuchuje na porcie 8889, ale **tylko lokalnie** (czyli dostępny jest tylko z tej samej maszyny).
- **Nie widzisz portów 4317 ani 4318** – czyli Twój Collector nie nasłuchuje na tych portach, albo jest inaczej skonfigurowany, albo nie działa.

---

## Co to znaczy w praktyce?

1. **Port 8889 działa** – możesz wejść na `http://localhost:8889/metrics` (na tej maszynie) i zobaczyć metryki eksportowane przez Collectora.
2. **OTLP (4317, 4318) nie działa** – Twój Collector nie nasłuchuje na tych portach, więc nie odbiera danych przez OTLP (ani gRPC, ani HTTP).

---

## Co zrobić, żeby OTLP działał?

- **Sprawdź konfigurację Collectora** – czy na pewno masz ustawione porty 4317 i 4318?
- **Upewnij się, że Collector jest uruchomiony z tą konfiguracją**
- **Sprawdź logi Collectora** – mogą być tam błędy dotyczące uruchamiania receiverów OTLP

---

## Podsumowanie

- **Port 8889** – działa, Collector nasłuchuje (możesz sprawdzić metryki Prometheus).
- **Brak portów 4317/4318** – OTLP nie działa, Collector nie nasłuchuje na tych portach.
- **Dostępność** – Collector na porcie 8889 jest dostępny tylko lokalnie (z tej samej maszyny).

---

**Jeśli chcesz, żebym pomógł Ci poprawić konfigurację lub sprawdzić, dlaczego nie działa OTLP, wklej swoją konfigurację Collectora i sposób jego uruchamiania!**