
# Wyjasninie:
```
echo "# HELP vyos_top_process_memory Top 5 processes by memory usage (RSS, in bytes)" >> "$TEMP_FILE"
echo "# TYPE vyos_top_process_memory gauge" >> "$TEMP_FILE"


ps -eo pid,comm,%mem,rss,%cpu,args --sort=-rss | head -n 6 | tail -n 5 | while read -r pid comm pmem rss pcpu args; do
    safe_args=$(echo "$args" | sed 's/"/'\''/g')
    echo "vyos_top_process_memory{pid=\"$pid\",name=\"$comm\",percent_mem=\"$pmem\",percent_cpu=\"$pcpu\",args=\"$safe_args\"} $((rss*1024))" >> "$TEMP_FILE"
done

echo "" >> "$TEMP_FILE"
```

---

## Co tu się dzieje?

### 1. Dodawanie nagłówków do pliku tymczasowego

```bash
echo "# HELP vyos_top_process_memory Top 5 processes by memory usage (RSS, in bytes)" >> "$TEMP_FILE"
echo "# TYPE vyos_top_process_memory gauge" >> "$TEMP_FILE"
```
- Te linie dodają komentarze/nagłówki do pliku tymczasowego (`$TEMP_FILE`), opisujące metrykę w stylu Prometheus.  
- **HELP – opisuje, co oznacza metryka.  
- **# TYPE określa typ metryki (tutaj: `gauge`).

---

### 2. Pobieranie i przetwarzanie procesów

```bash
ps -eo pid,comm,%mem,rss,%cpu,args --sort=-rss | head -n 6 | tail -n 5 | while read -r pid comm pmem rss pcpu args; do
    safe_args=$(echo "$args" | sed 's/"/'\''/g')
    echo "vyos_top_process_memory{pid=\"$pid\",name=\"$comm\",percent_mem=\"$pmem\",percent_cpu=\"$pcpu\",args=\"$safe_args\"} $((rss*1024))" >> "$TEMP_FILE"
done
```
- **ps -eo ... --sort=-rss** – wypisuje listę procesów wraz z:  
  - `pid` – ID procesu  
  - `comm` – nazwa polecenia  
  - `%mem` – procent użycia pamięci  
  - `rss` – ilość pamięci RAM (w kilobajtach)  
  - `%cpu` – procent użycia CPU  
  - `args` – pełna linia polecenia  
  - Sortuje malejąco po RSS (pamięci).
- **head -n 6 | tail -n 5** – pomija pierwszy wiersz (nagłówek) i wybiera 5 kolejnych (czyli top 5 procesów po zużyciu RAM).
- **while read ...; do ... done** – dla każdego z tych 5 procesów wykonuje pętlę:
    - **safe_args** – zamienia cudzysłowy (`"`) na pojedyncze apostrofy (`'`), by nie psuły składni Prometheus.
    - **echo ... >> "$TEMP_FILE"** – zapisuje linię w formacie Prometheus, gdzie:
        - `vyos_top_process_memory{...}` – etykiety z informacjami o procesie
        - `$((rss*1024))` – wartość RSS przeliczona na bajty (bo RSS jest w KB).

---

### 3. Dodanie pustej linii

```bash
echo "" >> "$TEMP_FILE"
```
- Dodaje pustą linię na końcu, dla czytelności.

---

## Podsumowanie

**Ten fragment skryptu generuje metryki Prometheus pokazujące 5 procesów zużywających najwięcej pamięci RAM na systemie (w bajtach), wraz z ich parametrami. Wynik jest zapisywany do pliku tymczasowego.**

Jeśli masz pytania o szczegóły lub chcesz rozwinąć ten skrypt, daj znać!