
czyli nie mozna tego zrealizowac w jednym load balancerze ?
Źródła
Odpowiedź
Nie, nie można zrealizować tego scenariusza w jednym load balancerze w Google Cloud Platform (GCP) w sposób, który pozwalałby na jednoczesne dodawanie nagłówków przez load balancer i przekierowywanie do innego hosta. W GCP, jeśli chcesz, aby ruch był najpierw przetwarzany przez backend (gdzie dodawane są nagłówki), a następnie przekierowywany do innego hosta, musisz zastosować inne podejście.
Dlaczego To Nie Działa w Jednym Load Balancerze?
Ograniczenia Konfiguracji: W GCP nie można zdefiniować reguły, która jednocześnie dodaje nagłówki do żądania i wykonuje przekierowanie URL. Reguły url_redirect nie mogą współistnieć z backend service w tej samej regule. Oznacza to, że musisz wybrać, czy chcesz przetwarzać ruch w backendzie, czy go przekierować.
Logika Backend: Aby osiągnąć zamierzony efekt (dodawanie nagłówków i przekierowanie), musisz zaimplementować logikę przekierowania bezpośrednio w aplikacji działającej na backendzie. Aplikacja może najpierw przetworzyć żądanie (dodając odpowiednie nagłówki), a następnie zwrócić odpowiedź z kodem przekierowania.
Możliwe Rozwiązania
Backend Service z Przekierowaniem:
Skonfiguruj backend service tak, aby obsługiwał logikę dodawania nagłówków oraz przekierowania. Aplikacja powinna zwracać odpowiedź z nowymi nagłówkami i kodem HTTP 302 (lub innym odpowiednim kodem przekierowania).
Użycie Google Cloud Armor:
Możesz również rozważyć użycie Google Cloud Armor do zarządzania politykami bezpieczeństwa i modyfikacji nagłówków odpowiedzi na podstawie warunków żądań. To podejście pozwala na bardziej zaawansowane operacje związane z nagłówkami, ale wymaga dodatkowej konfiguracji.
Przykład Logiki Backend
Oto przykład, jak możesz zaimplementować logikę w aplikacji backendowej:
python
from flask import Flask, redirect, make_response

app = Flask(__name__)

@app.route('/your-endpoint')
def your_endpoint():
    # Tworzenie odpowiedzi z nowymi nagłówkami
    response = make_response(redirect("http://host.com/some/path", code=302))
    response.headers['AddSomethingElse'] = 'MyOtherValue'
    response.headers['AddMe'] = 'MyValue'
    return response

Podsumowanie
Nie można realizować równocześnie: Nie można jednocześnie dodawać nagłówków i wykonywać przekierowań w jednej regule URL mapy.
Logika aplikacji: Przekierowanie musi być obsługiwane przez backend, który najpierw przetwarza żądanie.
Alternatywne podejścia: Rozważ użycie Google Cloud Armor lub innych mechanizmów do zarządzania nagłówkami.
Dzięki tym rozwiązaniom możesz osiągnąć zamierzony efekt w GCP, mimo ograniczeń związanych z konfiguracją load balancera.
