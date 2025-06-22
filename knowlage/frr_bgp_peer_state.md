Wartość **1** w polu stanu (state) BGP peera w FRR oznacza stan **Idle**. 

Stan Idle to początkowy stan BGP, w którym peer czeka na zdarzenie startowe i nie próbuje jeszcze nawiązać połączenia TCP z drugim routerem. Może to oznaczać, że sesja BGP jeszcze się nie rozpoczęła lub występuje problem z nawiązaniem połączenia (np. brak ścieżki do peera, błędna konfiguracja) i BGP czeka na ponowną próbę[1][2][4][7][9].

Podsumowanie wartości stanu BGP peera z odpowiadającymi im liczbami (przykład z biblioteki bgpstream):  
- 0 = Unknown (nieznany)  
- 1 = Idle (bezczynny, oczekiwanie)  
- 2 = Connect (próba połączenia TCP)  
- 3 = Active (aktywna próba połączenia)  
- 4 = OpenSent (wysłano OPEN, czekanie na OPEN od peera)  
- 5 = OpenConfirm (potwierdzenie OPEN, oczekiwanie na KEEPALIVE)  
- 6 = Established (sesja BGP ustanowiona, aktywna wymiana tras)[9]

Jeśli masz wartość 1, oznacza to, że sesja BGP jest w stanie Idle i nie jest jeszcze nawiązane połączenie z peerem. Warto wtedy sprawdzić dostępność sieciową, konfigurację peera i ewentualne błędy w logach.

Citations:
[1] https://networklessons.com/bgp/bgp-neighbor-adjacency-states
[2] https://support.huawei.com/enterprise/en/knowledge/ETC0001019646
[3] https://www.ciscopress.com/articles/article.asp?p=2756480&seqNum=4
[4] https://community.cisco.com/t5/network-management/invalid-bgp-peer-state-via-snmp/td-p/4766586
[5] https://learn.microsoft.com/zh-tw/dotnet/api/microsoft.azure.management.network.fluent.models.bgppeerstatus?view=azure-dotnet-legacy
[6] https://www.zframez.com/articles/routing/bgp-protocol-peering-and-states
[7] https://knowledgebase.paloaltonetworks.com/kcSArticleDetail?id=kA10g000000PLlk
[8] https://www.networkurge.com/2020/07/bgp-states.html
[9] https://bgpstream.caida.org/docs/api/libbgpstream/bgpstream_elem.h.html

---
Odpowiedź od Perplexity: pplx.ai/share