Oto podział elementów infrastruktury Google Cloud Platform (GCP) według poziomów modelu OSI:

## Elementy infrastruktury GCP według poziomów OSI

|**Poziom OSI**|**Elementy GCP**|**Opis**|
|---|---|---|
|**Poziom 1: Fizyczny**|Data centers, kable, przełączniki, routery|Fizyczne komponenty, które umożliwiają transmisję danych.|
|**Poziom 2: Łącza Danych**|Virtual Private Cloud (VPC), VLANs, ARP, PPP|Odpowiada za transfer ramek danych między bezpośrednio połączonymi urządzeniami.|
|**Poziom 3: Sieciowy**|Cloud Router, VPC routing, IP addressing|Zarządza trasowaniem pakietów i wyborem najlepszej ścieżki dla transmisji danych.|
|**Poziom 4: Transportowy**|Cloud Load Balancing, TCP/UDP|Zapewnia niezawodną dostawę danych między hostami oraz zarządza kontrolą przepływu i błędami.|
|**Poziom 5: Sesji**|API Gateway, Cloud Functions|Umożliwia tworzenie i zarządzanie sesjami komunikacyjnymi między aplikacjami.|
|**Poziom 6: Prezentacji**|Cloud Storage (kompresja danych), usługi szyfrowania|Odpowiada za formatowanie danych oraz ich przygotowanie do przesyłania między aplikacjami.|
|**Poziom 7: Aplikacji**|Google Compute Engine, Google Kubernetes Engine, API Google Cloud|Najwyższy poziom, który obsługuje interakcje użytkowników z aplikacjami oraz zarządza danymi.|

## Opis poziomów OSI

1. **Fizyczny (Layer 1)**: Obejmuje wszystkie fizyczne aspekty infrastruktury sieciowej, takie jak kable i urządzenia sieciowe.
2. **Łącza Danych (Layer 2)**: Zapewnia komunikację między urządzeniami w tej samej sieci lokalnej.
3. **Sieciowy (Layer 3)**: Odpowiada za trasowanie pakietów w sieci i zarządzanie adresowaniem IP.
4. **Transportowy (Layer 4)**: Umożliwia przesyłanie danych pomiędzy systemami oraz zapewnia niezawodność transmisji.
5. **Sesji (Layer 5)**: Umożliwia nawiązywanie i zarządzanie sesjami komunikacyjnymi.
6. **Prezentacji (Layer 6)**: Odpowiada za konwersję i formatowanie danych do odpowiednich standardów.
7. **Aplikacji (Layer 7)**: Obsługuje aplikacje użytkowe i interakcje z użytkownikami.

Taki podział pozwala zrozumieć, jak różne elementy GCP współdziałają w kontekście modelu OSI i jakie funkcje pełnią na każdym z poziomów.

***
***
***
Na poziomie 3 modelu OSI w Google Cloud Platform (GCP) znajdują się różne elementy, które odpowiadają za zarządzanie ruchem sieciowym i trasowaniem pakietów. Oto lista wszystkich kluczowych elementów GCP związanych z poziomem 3 OSI:

## Elementy GCP na poziomie 3 OSI

1. **VPC (Virtual Private Cloud)**:
    
    - Izolowane środowisko sieciowe, w którym można tworzyć i zarządzać zasobami.
    
2. **Maszyny Wirtualne (VM)**:
    
    - Instancje obliczeniowe, które komunikują się w obrębie VPC.
    
3. **Routery**:
    
    - Urządzenia odpowiedzialne za trasowanie ruchu sieciowego między różnymi podsieciami i VPC.
    
4. **Tablice routingu**:
    
    - Zestaw reguł określających, jak ruch jest kierowany w sieci. Każda VPC ma swoją tablicę routingu, która zawiera trasy do różnych zasobów.
    - **Typy tras**:
        
        - **Trasy domyślne**: Automatycznie tworzone dla każdej VPC.
        - **Trasy podsieci**: Tworzone dla każdej podsieci w VPC.
        - **Trasy statyczne**: Użytkownik może definiować własne trasy.
        - **Trasy dynamiczne**: Używane w połączeniu z protokołem BGP, umożliwiają automatyczne aktualizowanie tras na podstawie informacji o dostępnych trasach.
        
    
5. **Peering VPC**:
    
    - Umożliwia bezpośrednią komunikację między różnymi VPC, co pozwala na wymianę ruchu między nimi.
    
6. **VPN Gateway**:
    
    - Umożliwia bezpieczne połączenie między GCP a lokalnymi środowiskami lub innymi chmurami.
    
7. **Policy-based Routes**:
    
    - Trasy oparte na politykach, które są oceniane przed innymi typami tras i pozwalają na bardziej złożone reguły routingu.
    
8. **Dynamic Routing**:
    
    - Umożliwia automatyczne dostosowywanie tras w odpowiedzi na zmiany w topologii sieci.
    

## Opis funkcji

- **VPC i VM**: Tworzą podstawową infrastrukturę, gdzie zasoby są uruchamiane i komunikują się ze sobą.
- **Routery i tablice routingu**: Zarządzają tym, jak pakiety są kierowane do różnych zasobów w sieci.
- **Peering i VPN**: Umożliwiają komunikację z innymi sieciami oraz lokalnymi środowiskami.
- **Policy-based Routes i Dynamic Routing**: Umożliwiają bardziej elastyczne zarządzanie ruchem sieciowym.

Te elementy współpracują ze sobą, aby zapewnić efektywne zarządzanie ruchem sieciowym w GCP, co jest kluczowe dla działania aplikacji i usług działających w chmurze.