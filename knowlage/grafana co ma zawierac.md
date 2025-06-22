

grafana to do
what need be included


1. czy uzadzaenie zyje -> czy nie na interfacach
2. jak ruch sie odbywa ile megabitow/s + linia maks ( ile zostalo do maxa)
3. ver os-a
4. uptime
5. ram
6. informacje na temat routingu, jakie rout sa rozglaszane itd


OS,
Kernel
CPU cores
Uptime
RAM total
Root fs

System
 cpu usage


 20strona


 Odnośnie grafany - zbieramy wszystkie metryki opisane w dokumencie, który przesłałem, należy przygotować 2 Dashboardy - jeden podobny do tego, który już jest gdzie mamy informacje o sieci (jako sekcja network- z możliwością zwijania - każda sekcja może być zwinięta), tam mamy interfejsy podzielone na 3 sekcje w zaleznosci od typu interfejsu czyli gdzie on jest podlaczony, cpu memory storage - kolejna sekcja, routing - kokejna itd w zależności od metryk, które są w dokumentacji zebrane. Dodatkowo mamy 2 Dashboard gdzie mamy możliwość zbiorczego wybrania kilku instancji i zaprezentowania danych w formie zbiorczej wybiarajac dana instancję, tutaj bedzie jeszcze parametr up/down do monitorowania oraz logi - errory i war.

[] zbieramy wszystkie metryki opisane w dokumencie
[] należy przygotować 2 Dashboardy 
[] jeden podobny do tego, który już jest gdzie mamy informacje o sieci (jako sekcja network- z możliwością zwijania - każda sekcja może być zwinięta)
[] interfejsy podzielone na 3 sekcje w zaleznosci od typu interfejsu czyli gdzie on jest podlaczony, cpu memory storage - kolejna sekcja, routing - kokejna itd w zależności od metryk, które są w dokumentacji zebrane.
[] 2 Dashboard gdzie mamy możliwość zbiorczego wybrania kilku instancji i zaprezentowania danych w formie zbiorczej wybiarajac dana instancję, tutaj bedzie jeszcze parametr up/down do monitorowania oraz logi - errory i war.

15.04.2025

grafana

overviev na urzadzenia

LAN, CALY TRAFFIC, ERRORS ON INTERFACE, DYSK

oveirvev zbiorczy

taby dla wszystkich urzadzen, mozliwosc kilku urzdzen


plan na grafana
1. poc pododawac wszystko
2. dashboardy
3. vyos co mozna
4. health-check

vyos - spradzic czy mozna wszystko sprawdzic

health-check 

sekcje 
ram
dysk
itd
uptime
routing caly