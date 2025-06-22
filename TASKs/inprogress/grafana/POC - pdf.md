
#grafana #poc

2. Opis
Ta strona będzie używana do dokumentowania wszystkich aspektów Observability PoC (Proof of Concept - dowód koncepcji) w oparciu o dokumentację architektury dostępną pod następującym linkiem: Zonal Observability POC1.

2.1 Konstrukcje sieciowe

2.1.1 Serwer Syslog
W chwili pisania tego dokumentu serwer Syslog będzie wdrożonym klastrem K8s. Szczegóły dotyczące klastra znajdują się w dokumentacji architektury.
EDP - Infrastruktura – POC - Komunikacja Syslog dla Observability.
ACI Fabric – 6

3. ACI Fabric
Poniższa dokumentacja opisuje implementację SYSLOG i związane z nią aspekty w środowisku Cisco ACI Fabric. W bieżącym wdrożeniu kontroler APIC i przełączniki FABRIC działają w wersji 5.2(8i), więc cała dokumentacja odnosi się do tej konkretnej wersji oprogramowania, ponieważ na niej będzie przeprowadzane PoC.

Na początek, poniższe linki są udostępniane jako odniesienie od Cisco dla konfiguracji SYSLOG:
=>
https://www.cisco.com/c/en/us/td/docs/dcn/aci/apic/5x/basic-configuration/cisco-apic-basicconfiguration-
guide-52x/m_management.html
https://www.cisco.com/c/en/us/td/docs/dcn/aci/apic/5x/aci-fundamentals/cisco-aci-fundamentals-52x/
monitoring-52x.html

3.1 KROKI KONFIGURACJI SYSLOG ACI FABRIC
*** Integracja ACI DT AZ1 FABRIC SYSLOG zostanie zaktualizowana z powodu przeniesienia bramy observability do środowiska DT AZ2. Na razie ta integracja jest wstrzymana ***

Dane WEJŚCIOWE zebrane i istotne dla konfiguracji są następujące:
Serwer Syslog (serwer observability) to => obsgw.ctn-h.test.pndrs.de
Serwer Syslog => c1.ctn-h.test.pndrs.de
Używany protokół => syslog (RFC5424)
Używany port TCP => tcp/54526
Adresy IP ACI FABRIC APIC => 100.126.116.35, 100.126.116.37, 100.126.116.38

Aby ACI FABRIC mógł wysyłać wiadomości syslog, należy dostarczyć trzy główne części konfiguracji:
Utwórz miejsce docelowe Syslog i grupę miejsc docelowych
Utwórz źródło Syslog
Włącz reguły bezpieczeństwa w infrastrukturze (patrz sekcja reguł bezpieczeństwa poniżej)

***
***
### Wytłumaczenie pojęć w/w :

Observability PoC (Proof of Concept) - Dowód koncepcji dotyczący "obserwowalności" (ang. observability), czyli zdolności do zrozumienia wewnętrznego stanu systemu na podstawie danych wyjściowych. ==Celem PoC jest sprawdzenie, czy dana architektura lub rozwiązanie do monitorowania i analizy danych działa zgodnie z założeniami.==

K8s - Skrót od Kubernetes, czyli platformy do automatyzacji wdrażania, skalowania i zarządzania aplikacjami w kontenerach. Kubernetes pozwala na efektywne zarządzanie zasobami i zapewnia wysoką dostępność aplikacji.

Syslog - Standardowy ==protokół używany do przesyłania komunikatów o zdarzeniach systemowych i logów== z różnych urządzeń sieciowych do centralnego serwera. Umożliwia centralne zbieranie i analizowanie logów, co ułatwia diagnozowanie problemów.

ACI Fabric - Rozwiązanie Cisco do tworzenia programowalnych sieci w centrach danych. ACI (Application Centric Infrastructure) Fabric umożliwia automatyzację zarządzania siecią i optymalizację wydajności aplikacji.

APIC controller - Centralny kontroler w architekturze Cisco ACI, który zarządza i konfiguruje sieć. APIC (Application Policy Infrastructure Controller) odpowiada za definiowanie polityk sieciowych i zapewnianie spójnej konfiguracji w całej infrastrukturze.

DT AZ1/AZ2 - Prawdopodobnie oznaczenia stref dostępności (Availability Zones) w konkretnym środowisku (DT). Strefy dostępności to fizycznie oddzielone lokalizacje w regionie chmury, które zapewniają odporność na awarie.

Namespace - W Kubernetes, przestrzeń nazw (namespace) to sposób na logiczne podzielenie zasobów klastra. Pozwala na izolację aplikacji i zarządzanie dostępem do zasobów w klastrze.

Pod - Najmniejsza, wdrażalna jednostka w Kubernetes, która zawiera jeden lub więcej kontenerów. Pody współdzielą zasoby sieciowe i pamięci, co umożliwia bliską współpracę między kontenerami w ramach jednej aplikacji.

Workload - W Kubernetes, workload to aplikacja lub zestaw aplikacji uruchomionych w klastrze. Workloady są zarządzane przez platformę i przypisywane do przestrzeni nazw.

DaemonSet - W Kubernetes, kontroler, który zapewnia, że kopia Poda jest uruchomiona na każdym węźle w klastrze. DaemonSet jest używany do wdrażania agentów monitoringu, logowania lub innych usług działających na każdym węźle.

Metrics-server - Serwer metryk zbierający dane o zużyciu CPU i pamięci przez kontenery.

***
***
