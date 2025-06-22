
Oto szczegółowe wyjaśnienie Twojej polityki IAM krok po kroku:

Struktura polityki
json
{
  "Version": "2012-10-17",
  "Statement": [
    // ... trzy zapisy (statements) ...
  ]
}
1. Uprawnienia do CloudWatch Logs
json
{
  "Effect": "Allow",
  "Action": "logs:CreateLogGroup",
  "Resource": "arn:aws:logs:us-east-1:992382768587:*"
}
Co to robi?
Pozwala Lambda na tworzenie nowych grup logów w CloudWatch.

logs:CreateLogGroup – tworzy grupę logów (np. /aws/lambda/nazwa-funkcji).

Resource: Dotyczy wszystkich grup logów w Twoim koncie (992382768587) w regionie us-east-1.

Przykład użycia:
Gdy uruchamiasz Lambdę po raz pierwszy, AWS automatycznie tworzy grupę logów dla tej funkcji.

2. Uprawnienia do zapisu logów
json
{
  "Effect": "Allow",
  "Action": ["logs:CreateLogStream", "logs:PutLogEvents"],
  "Resource": ["arn:aws:logs:us-east-1:992382768587:log-group:/aws/lambda/get-usage_plan:*"]
}
Co to robi?
Pozwala Lambda na zapisywanie konkretnych logów do CloudWatch.

logs:CreateLogStream – tworzy strumień logów wewnątrz grupy.

logs:PutLogEvents – zapisuje zdarzenia (np. console.log("test")) do strumienia.

Resource: Dotyczy tylko grupy logów Twojej funkcji Lambda (/aws/lambda/get-usage_plan).

Przykład użycia:
Każde wywołanie Lambdy generuje nowy strumień logów z informacjami o wykonaniu.

3. Uprawnienia do API Gateway
json
{
  "Effect": "Allow",
  "Action": ["apigateway:GET"],
  "Resource": [
    "arn:aws:apigateway:us-east-1::/usageplans",
    "arn:aws:apigateway:us-east-1::/usageplans/*",
    "arn:aws:apigateway:us-east-1::/usageplans/*/keys",
    "arn:aws:apigateway:us-east-1::/usageplans/*/keys/*"
  ]
}
Co to robi?
Pozwala Lambda na odczyt informacji o usage plans i kluczach API w API Gateway.

apigateway:GET – pozwala na wywołanie metod GET w API Gateway.

Zasoby:

/usageplans – lista wszystkich planów użycia.

/usageplans/* – szczegóły konkretnego planu (np. /usageplans/abc123).

/usageplans/*/keys – lista kluczy przypisanych do planu.

/usageplans/*/keys/* – szczegóły konkretnego klucza.

Przykład użycia w kodzie:

python
client.get_usage_plans()          # Wymaga uprawnienia do /usageplans
client.get_usage_plan_keys(usagePlanId="abc123")  # Wymaga uprawnienia do /usageplans/abc123/keys
Podsumowanie w tabeli
Sekcja	Uprawnienia	Cel
CloudWatch Logs	Tworzenie grup logów	Automatyczne logowanie dla nowych funkcji Lambdy
CloudWatch Logs	Zapis do strumieni logów	Przechwytywanie logów z wykonania funkcji
API Gateway	Odczyt planów użycia i kluczy	Sprawdzanie, do jakiego planu należy klucz API
Dlaczego ta polityka jest potrzebna?
Twoja Lambda wywołuje metody get_usage_plans() i get_usage_plan_keys(), które komunikują się z API Gateway. Bez tych uprawnień AWS blokuje dostęp, zgłaszając błąd AccessDeniedException.

Poprzedni błąd:

text
User [...] is not authorized to perform: apigateway:GET on resource: [...]/usageplans
został rozwiązany dzięki dodaniu trzeciej sekcji polityki.