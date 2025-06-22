#keycloak 

start : 
```
docker run -p 8080:8080 -e KC_BOOTSTRAP_ADMIN_USERNAME=admin -e KC_BOOTSTRAP_ADMIN_PASSWORD=admin quay.io/keycloak/keycloak:26.1.3 start-dev
```
id : 318bca9a-2e1a-4d67-8401-b4e10238b6a3
pass : qwer1234

![[Pasted image 20250310160401.png]]

JWT token

```

### Wartości standardowe JWT

exp: Data wygaśnięcia ważności tokenu, podana w sekundach od 1 stycznia 1970 roku (w formacie NumericDate). W tym przypadku token wygasa o godzinie odpowiadającej sekundzie 17416193191.

iat: Data utworzenia tokenu, również w sekundach od 1 stycznia 1970 roku1.

jti: Unikalny identyfikator JWT, który może być wykorzystany do uniknięcia wielokrotnego użycia tego samego tokenu1.

iss: Emitent tokenu, czyli podmiot, który wydał token. W tym przypadku jest to http://localhost:8080/realms/my-realm1.

aud: Odbiorca tokenu, czyli podmiot, dla którego token jest przeznaczony. Tutaj jest to account1.

sub: Podmiot, któremu token został wydany, w tym przypadku 318bca9a-2e1a-4d67-8401-b4e10238b6a31.

### Wartości specyficzne dla Keycloak

typ: Typ tokenu, w tym przypadku Bearer, co oznacza, że token powinien być używany w nagłówku Authorization z prefiksem Bearer2.

azp: Nazwa klienta, który zażądał tokenu, tutaj public-client2.

sid: Sesja użytkownika, identyfikator sesji2.

acr: Poziom uwierzytelnienia, w tym przypadku 12.

allowed-origins: Lista dozwolonych źródeł, z których można korzystać z tokenu. Tutaj jest to https://www.keycloak.org/2.

realm_access: Dostęp do ról w ramach realm. Zawiera listę ról, do których użytkownik ma dostęp, takie jak offline_access, uma_authorization, default-roles-my-realm3.

resource_access: Dostęp do ról dla konkretnych zasobów. Tutaj użytkownik ma role manage-account, manage-account-links, view-profile dla zasobu account3.

scope: Zakres uprawnień, które token zapewnia. W tym przypadku są to openid, email, profile2.

###Dane użytkownika

email_verified: Czy adres e-mail użytkownika został zweryfikowany, tutaj true2.

name: Pełne imię i nazwisko użytkownika, w tym przypadku jan dywan2.

preferred_username: Preferowany login użytkownika, tutaj jandywan@test.com2.

given_name: Imię użytkownika, tutaj jan2.

family_name: Nazwisko użytkownika, tutaj dywan2.

email: Adres e-mail użytkownika, tutaj jandywan@test.com2.
```


***
