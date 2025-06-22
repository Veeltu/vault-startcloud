

## Top 5 procesów według użycia CPU (`vyos_top_process_cpu`)

Każda linia ma następującą strukturę:
```
vyos_top_process_cpu{name="...",percent_cpu="...",percent_mem="...",pid="..."} ...
```
Wyjaśnienie poszczególnych wartości:

- **name** – nazwa procesu (np. `"kworker/0:1-eve"`, `"sshd"`, `"node_exporter"`). To identyfikator procesu, który pozwala rozpoznać, jaka aplikacja lub usługa zużywa zasoby.
- **percent_cpu** – procent użycia CPU przez dany proces, mierzony w krótkim oknie czasowym (zwykle 1 sekunda). Wartość ta pokazuje, jaką część czasu procesora dany proces wykorzystał w ostatnim okresie pomiarowym. Na przykład, wartość `50.0` oznacza, że proces używał 50% jednego rdzenia CPU w tym czasie[3][5].
- **percent_mem** – procent użycia pamięci RAM przez proces, liczony względem całkowitej dostępnej pamięci RAM. Pokazuje, ile procent pamięci operacyjnej zajmuje ten proces.
- **pid** – identyfikator procesu (Process ID), unikalny numer przypisany do każdego uruchomionego procesu w systemie.
- **wartość na końcu linii** – powtarza wartość `percent_cpu` w formacie liczbowym, zgodnie z konwencją Prometheus (np. `0.3`, `50`).

Przykład:
```
vyos_top_process_cpu{name="ps",percent_cpu="50.0",percent_mem="0.2",pid="44601"} 50
```
- Proces `ps` używał 50% CPU i 0,2% pamięci RAM, jego PID to 44601.

## Top 5 procesów według użycia pamięci (`vyos_top_process_memory`)

Każda linia ma następującą strukturę:
```
vyos_top_process_memory{name="...",percent_cpu="...",percent_mem="...",pid="..."} ...
```
Wyjaśnienie poszczególnych wartości:

- **name** – nazwa procesu (np. `"python3"`, `"systemd-journal"`).
- **percent_cpu** – procent użycia CPU przez proces, analogicznie jak wyżej.
- **percent_mem** – procent użycia pamięci RAM przez proces.
- **pid** – identyfikator procesu.
- **wartość na końcu linii** – ilość pamięci zajmowanej przez proces w bajtach (RSS – Resident Set Size). RSS to ilość pamięci fizycznej aktualnie zajmowanej przez proces w RAM, bez uwzględnienia pamięci wymienionej (swap) czy zmapowanych, ale nieużywanych stron[4].

Przykład:
```
vyos_top_process_memory{name="python3",percent_cpu="0.0",percent_mem="4.8",pid="643"} 1.01482496e+08
```
- Proces `python3` zużywa 0% CPU, 4,8% pamięci RAM, ma PID 643 i zajmuje ok. 101 MB pamięci RAM (RSS).

## Podsumowanie

- **name** – nazwa procesu
- **percent_cpu** – procent użycia CPU przez proces (w krótkim oknie czasowym)
- **percent_mem** – procent użycia pamięci RAM przez proces
- **pid** – identyfikator procesu
- **wartość końcowa**:
  - dla `vyos_top_process_cpu` – powtórzenie procentu CPU w formacie liczbowym
  - dla `vyos_top_process_memory` – ilość zajmowanej pamięci RAM w bajtach (RSS)[4]

Te metryki pozwalają szybko zidentyfikować procesy najbardziej obciążające system pod względem CPU i pamięci RAM[2][3][4].

[1] https://forum.vyos.io/t/1-3-rc1-performance-regression-high-cpu-usage/6749
[2] https://unix.stackexchange.com/questions/4999/how-to-find-which-processes-are-taking-all-the-memory
[3] https://superuser.com/questions/41223/what-does-the-percent-cpu-mean
[4] https://stackoverflow.com/questions/7880784/what-is-rss-and-vsz-in-linux-memory-management
[5] https://www.scoutapm.com/blog/understanding-linuxs-cpu-stats
[6] https://forum.vyos.io/t/performance-question-for-tcp-throughput/14280
[7] https://www.reddit.com/r/vyos/comments/refiq7/monitoring_vyos/
[8] https://forum.vyos.io/t/anomaly-with-high-cpu-usage-during-ping-flood/11686
[9] https://forum.vyos.io/t/vyos-cached-buffer-memory-escalation/14002
[10] https://github.com/vyos/vyos-documentation/blob/current/docs/configuration/trafficpolicy/index.rst