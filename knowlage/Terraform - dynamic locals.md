## Wyjaśnienie Wyrażenia: `local.headers[path_matcher.value.path_matcher].headers.request_headers_to_add`

local.host_rules[path_matcher.value.path_matcher].host_rules.redirect_path

To wyrażenie w Terraformie służy do dynamicznego uzyskiwania dostępu do nagłówków żądania, które mają być dodawane w konfiguracji mapy URL. Oto szczegółowe wyjaśnienie, jak to działa:

## 1. **`local.headers`**

- **Definicja**: `local.headers` to lokalna zmienna, która przechowuje zdefiniowane nagłówki dla różnych matcherów ścieżek. Jest to mapa, która łączy nazwy matcherów z ich odpowiednimi nagłówkami.
- **Przykład**: W twojej konfiguracji masz dwa matchery: `path-matcher-family01` i `path-matcher-family02`, z różnymi nagłówkami dla każdego z nich.

## 2. **`path_matcher.value.path_matcher`**

- **Definicja**: `path_matcher.value.path_matcher` to odniesienie do aktualnie przetwarzanego matchera ścieżki w dynamicznej pętli. Zwraca nazwę matchera, na przykład `"path-matcher-family01"` lub `"path-matcher-family02"`.
- **Działanie**: Gdy Terraform iteruje przez różne matchery ścieżek, ta część wyrażenia pozwala na uzyskanie nazwy matchera dla bieżącej iteracji.

## 3. **Dostęp do Nagłówków**

- **Wyrażenie**: `local.headers[path_matcher.value.path_matcher]`
    
    - To wyrażenie uzyskuje dostęp do nagłówków przypisanych do konkretnego matchera ścieżki na podstawie jego nazwy. Na przykład, jeśli `path_matcher.value.path_matcher` jest równe `"path-matcher-family01"`, to wyrażenie zwróci wszystkie nagłówki związane z tym matcherem.
    
```
locals.host_rules[]
```
## 4. **Dostęp do Nagłówków Żądania**

- **Wyrażenie**: `local.headers[path_matcher.value.path_matcher].headers.request_headers_to_add`
    
    - Tutaj uzyskujesz dostęp do listy nagłówków żądania (`request_headers_to_add`) dla aktualnie przetwarzanego matchera. Ta lista zawiera obiekty, które definiują nazwę nagłówka, wartość oraz informację o tym, czy istniejący nagłówek powinien być zastąpiony.