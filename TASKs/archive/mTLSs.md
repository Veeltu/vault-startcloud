
- jakie certy ktore sa gdzie jak sie ze soba łączą jaka jest korelacja, bo jest kilka etapow.
- TLC cert musi miec CA, czyli cert autority + autoryzacja przez 2 strony i jak wyglada ten ruch
- wytlumaczyc jak dla dziecka, pod kątem GCP w kontekscie load balancera jak to ustawic
- overview, co jest potrzebne w podstawowej configuracji czyli 3 certy
- cert CA podpisuje certy + cert client + cert server
- prosty rys co sie z czym komunikuje
- prosty przyklad na 2 VM lub LB by przetestowac
- cert z opcją SAN + wildcard ( czyli zawiera wszystko, czyli jak damy server wp.pl i damy cert wp.pl to nadal bedzie dzialal O_O)
- cert powinien byc skonfigurowany na AWS na LB global ( tym publicznym) wymaga to skonfigurowania CA i CA po stronie LB czyli zeby klient i server mialy te certy mTLS spojne
- mozna dla przykladu zrobic miedzy AWS a GCP
- miedzy swietami na spokojnie




## Wprowadzenie do certyfikatów SSL/TLS w GCP i AWS

Certyfikaty SSL/TLS są kluczowe dla zabezpieczania komunikacji w Internecie. W kontekście Google Cloud Platform (GCP) i Amazon Web Services (AWS), zrozumienie, jak te certyfikaty współdziałają, jest niezbędne do prawidłowej konfiguracji load balancerów oraz zapewnienia bezpiecznej komunikacji.

## Typy certyfikatów i ich zależności

1. **Certyfikat CA (Certificate Authority)**: Wydawany przez zaufany urząd certyfikacji, jest podstawą dla wszystkich innych certyfikatów.
2. **Certyfikat serwera**: Umożliwia szyfrowanie połączeń z klientami. Jest podpisywany przez CA.
3. **Certyfikat klienta**: Używany do weryfikacji tożsamości klienta w połączeniach mTLS.

~={red}## Łańcuch zaufania ?????????

- **Root CA**: Główny certyfikat, który jest zaufany przez przeglądarki.
- **Intermediate CA**: Certyfikaty pośrednie, które łączą Root CA z certyfikatami końcowymi.
- **Certyfikat jednostki końcowej**: Certyfikat SSL/TLS dla konkretnej domeny.

## Ruch i autoryzacja w mTLS

W przypadku mTLS (mutual TLS), zarówno klient, jak i serwer muszą posiadać certyfikaty. Proces wygląda następująco:

1. **Klient łączy się z serwerem**: Serwer wysyła swój certyfikat do klienta.
2. **Weryfikacja certyfikatu serwera**: Klient sprawdza, czy certyfikat serwera jest ważny i podpisany przez zaufane CA.
3. **Wymiana certyfikatów**: Klient wysyła swój certyfikat do serwera, który również go weryfikuje.

## Konfiguracja GCP z użyciem load balancera

## Podstawowa konfiguracja

Aby skonfigurować load balancer na GCP, potrzebne są trzy podstawowe certyfikaty:

1. Certyfikat CA
2. Certyfikat serwera
3. Certyfikat klienta

## Proces konfiguracji

1. **Generowanie CSR**: Użyj OpenSSL do wygenerowania CSR (Certificate Signing Request).bash
    
    `openssl req -out CSR.csr -new -newkey rsa:2048 -nodes -keyout privateKey.key`
    
2. **Zamówienie certyfikatu**: Poproś o certyfikat od CA.
3. **Instalacja certyfikatu na load balancerze**: Po uzyskaniu certyfikatu, dodaj go do konfiguracji load balancera w Google Cloud Console[
## Rysunek ilustrujący komunikację


`[Klient] <--> [Load Balancer AWS] <--> [VM2]                   |                  v              [VM1 GCP]`

## Przykład testowania konfiguracji mTLS między AWS a GCP

## Wymagana konfiguracja

- Certyfikaty muszą być skonfigurowane zarówno na AWS (load balancer globalny), jak i GCP.
- Klient i serwer muszą posiadać odpowiednie certyfikaty.

## Testowanie komunikacji

1. Utwórz dwa VM (jeden na GCP, drugi na AWS).
2. Zainstaluj odpowiednie certyfikaty na każdym VM.
3. Skonfiguruj load balancer na AWS do przekazywania ruchu do VM na GCP.
4. Sprawdź komunikację między VM za pomocą narzędzi takich jak `curl` lub `Postman`.

## Użycie certyfikatów SAN i wildcard

Certyfikaty SAN (Subject Alternative Name) oraz wildcard pozwalają na zabezpieczenie wielu subdomen lub całych domen w ramach jednego certyfikatu. Na przykład:

- Certyfikat wildcard dla `*.wp.pl` będzie działał dla `server.wp.pl`, `api.wp.pl`, itp.

Dzięki tym rozwiązaniom można efektywnie zarządzać bezpieczeństwem komunikacji w chmurze oraz integrować różne platformy chmurowe, takie jak AWS i GCP, co jest szczególnie istotne w kontekście mTLS i load balancerów globalnych.

***

 check open ports: 
 sudo ss -tln


***
It takes the public key of the server and signs (generates digital signature) with his private key so that any clients can decrypt the content and verify if the public keys are the same to trust the server. Love to learn more from you.


The CA **verifies the certificate applicant's identity and issues a certificate containing their public key**. The CA will then digitally sign the issued certificate with their own private key which establishes trust in the certificate's validity