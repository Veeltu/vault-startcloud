## Wyjaśnienie fragmentu `volume { ... }` z projekcją sekretów TLS

Ten fragment kodu Terraform definiuje wolumen typu *projected* w specyfikacji zasobu Kubernetes (np. w Podzie lub Deploymentcie). Jego celem jest udostępnienie w kontenerze certyfikatów TLS (klucza prywatnego i certyfikatu) jako plików, które pochodzą z różnych sekretów Kubernetes.

**Co się dzieje w tym kodzie?**

- **volume { name = "secrets" ... }**  
  Tworzy wolumen o nazwie "secrets".

- **projected { ... }**  
  Używa wolumenu typu *projected*, który pozwala łączyć wiele źródeł (np. różne sekrety) w jeden wolumen montowany w kontenerze jako katalog.

- **sources { dynamic "secret" { ... } }**  
  Dynamiczna pętla *for_each* iteruje po wszystkich domenach z `local.certs`. Dla każdej domeny:
    - Tworzy źródło typu *secret* (sekret Kubernetes).
    - `name = replace(secret.value, ".", "-")` — nazwa sekretu odpowiada domenie, ale z zamienionymi kropkami na myślniki (tak samo jak w poprzednim kodzie z cert-managerem).
    - **items { ... }** — określa, które klucze z sekretu mają być udostępnione w wolumenie i pod jakimi nazwami plików:
        - `key = "tls.crt"` — certyfikat X.509, plik będzie miał nazwę np. `gw-observability-test-pndrs-de.crt`
        - `key = "tls.key"` — klucz prywatny, plik będzie miał nazwę np. `gw-observability-test-pndrs-de.key`

### Efekt końcowy

- W katalogu montowanym w kontenerze (np. `/mnt/secrets`), pojawią się pliki:
    - `gw-observability-test-pndrs-de.crt`
    - `gw-observability-test-pndrs-de.key`
  (i analogicznie dla każdej domeny z `local.certs`).
- Pliki te pochodzą z odpowiednich sekretów Kubernetes, które zostały wcześniej utworzone przez cert-managera i zawierają certyfikat oraz klucz prywatny.

---

### Do czego to służy?

- Umożliwia aplikacji dostęp do wielu certyfikatów TLS (i ich kluczy) w jednym miejscu, bez konieczności ręcznego kopiowania plików czy zarządzania nimi poza Kubernetes.
- Automatyzuje i standaryzuje sposób udostępniania certyfikatów w kontenerach, co jest szczególnie przydatne w środowiskach, gdzie obsługiwanych jest wiele domen lub endpointów.

---

### Podsumowanie

Kod ten:
- Tworzy wolumen typu *projected*, który łączy sekrety TLS dla wielu domen.
- Dla każdej domeny z `local.certs` udostępnia w kontenerze dwa pliki: certyfikat i klucz prywatny, z odpowiednimi nazwami.
- Pozwala aplikacjom na łatwy i bezpieczny dostęp do certyfikatów bezpośrednio z systemu plików w kontenerze.