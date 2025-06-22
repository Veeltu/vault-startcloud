
podzial na aplikacje czy jest mozliwe ?
podzial na produty czy jest mozliwy

czy mozna jedno proxy z podzialem na aplikacje
i czy mozna zrobic to podobnie bez pisania od nowe

logika dekodowania id/klucza czy lambda czy GATE

czydostep do digital-a brac do szkolenia ? odpowiedz tak, poniewaz powinienem sie uczyc
***

Pytania:
- Koszty lambdy oszacować
- Czy scieki mogą być takie same w api-Gateway (np: /pets; /pets; )
- Sciezki do target-sevice-url, variables gdzie trzymać, żeby było to dynamicznie w jednym miejscu ?
- API-gateway: czy można zrobić metody/ konsolidacje w jedno żeby nie trzeba było w wszystkiego za każdym razem ręcznie ustawiać w API Gateway
- W jaki sposób logowanie do konsoli jak error albo logi do weryfikacji
- Autoryzacja - kilok per/metoda, do serwisu TLS JWT, lambda-service

## Q: Koszty lambdy oszacować
założenia:
    60 lambd / nie ma znaczenia,
    5 sek 
    2 gb
    200 zadan/sek - 
    miesiąc ma 2 600 000 sekund,
    520 000 000 req miesięcznie 


AWS Lambda Pricing Calculator:


Number of Executions 520000000
Allocated Memory (MB) 2048
Estimated Execution Time (ms) 5000

Execution Costs: $86677.33
________________

Number of Executions 520000000
Allocated Memory (MB) 1408
Estimated Execution Time (ms) 5000

Execution Costs: $59588.58
________________

Number of Executions 520000000
Allocated Memory (MB) 1408
Estimated Execution Time (ms) 2000
Execution Costs: $23831.43
***

test wydajności pamięci :
https://medium.com/@raupach/choosing-the-right-amount-of-memory-for-your-aws-lambda-function-99615ddf75dd

*można zrobić podobny test już z docelowym skryptem

## Q: Czy scieki mogą być takie same w api-Gateway (np: /pets; /pets; ) - NIE

## Q: Sciezki do target-sevice-url, variables gdzie trzymać, żeby było to dynamicznie w jednym miejscu ?
1. as Environment Variables 
    - its easy to update using AWS SKD CLI tool
    
 	example : https://medium.com/@haider.mtech2011/dynamically-managing-aws-lambda-layers-and-environment-variables-a-comprehensive-guide-49352491ac03

 2. as aws-s3 - its possible but this solution is not design for it.

 3. Parameter Store 
     Parameter Store allows you to create key-value parameters to save your application configurations, custom environment variables, product keys, and credentials on a single interface. 
     
	- **main way to store configs in AWS**
    - key-value
	- store and access application configuration globaly in AWS
	- can be string, string list, secure string ( KMS )
	- history of values is retained
	- two type :
		- standard :
			- 4kb max size
			- cannot use Parameter Polices
			- free
		- advance :
			- 8kb max size
			- can use Parameter Polices
			- charges Apply ---> ($0.05 per parameter / month)
		* can upgrade, cant downgrade

	- guarded by IAM
	- limits : default 40 TPS, max: 3000 TPS (transactions per sec)
	- * check lambda extansion for caching parameters

``` example
response = client.get_parameter(Name="SecretString", WithDecryptions=True)
```
*example in py*
    
	officaldocs : https://docs.aws.amazon.com/systems-manager/latest/userguide/ps-integration-lambda-extensions.html
	good yt-vid about : https://www.youtube.com/watch?v=8Hstqmge71w

 1. Secret Manager 
        AWS Secrets Manager enables you to rotate, manage, and retrieve database credentials, API keys and other secrets throughout their lifecycle. It also makes it really easy for you to follow security best practices such as encrypting secrets and rotating these regularly. 
     
     - KMS encryption
     - access is managed through IAM and monitored via Cloudwatch, and Cloudtrial
     - can work with big numebers of data
     - **centralizes secret storage and encrypts data**
     - allows control the lifecycle of secrets including rotation
     - up to 64Kb secret size
     - can’t store data in plaintext in Secrets Manager.

     - pricing:
         - 30 first day free trial period
         - 0.4 $ per secret per month
         - 0.5 $ per 10'000 GetSecretValue API calls
         - caching is possible with secrets
         
 ```example
 API_ACCESS_TOKEN = secret_manage_respone['SecretString']['ApiKey']
 client.call_api(API_ACCESS_TOKE,...)
 ```
*example*

*diff Parameter Store vs Secret Manager:* https://tutorialsdojo.com/aws-secrets-manager-vs-systems-manager-parameter-store/


## Q: W jaki sposób logowanie do konsoli jak error albo logi zobaczyć

CloudWatch i CloudTrail są używane do monitorowania i rejestrowania aktywności, ale CloudWatch koncentruje się na wydajności i kondycji zasobów, a CloudTrail na bezpieczeństwie i zgodności.

1. Amazon CloudWatch
    Metryki: CloudWatch zbiera metryki dotyczące API Gateway, takie jak liczba wywołań API, opóźnienia, błędy 4XX i 5XX. Można tworzyć alarmy, które powiadamiają o przekroczeniu określonych progów
    
    Logi: CloudWatch Logs umożliwia zbieranie i analizę logów z API Gateway, co pomaga w diagnozowaniu błędów

    Dashboardy: Można tworzyć spersonalizowane dashboardy do wizualizacji kluczowych metryk

2. AWS CloudTrail
    Zapewnia rekordy wszystkich akcji wykonywanych w API Gateway, w tym informacje o żądaniach, adresach IP i użytkownikach

3. Amazon SNS - powiadomienia aws
    Można go użyć do otrzymywania powiadomień o alarmach CloudWatch


## Q: Autoryzacja - do serwisu TLS JWT, lambda-service



Tokeny JWT mogą być tworzone i obsługiwane przez AWS-Cognito, następnie w samej lambdzie możemy wykorzystać token za pomoca oficjalnych bibliotek jak aws-jwt-verify


W przypadku korzystania z tokenów JWT z usług zewnętrznych, można wykorzystać AWS Lambda do weryfikacji.


## Q: API-gateway: czy można zrobić metody/ konsolidacje w jedno żeby nie trzeba było w wszystkiego za każdym razem ręcznie ustawiać w API Gateway

chodzi o grupowanie 

terraform/cloud formation/ bash script ?


***

API klucze - gateway, jak zarzadzac z uwzglednieniem sciezek.  - operator - 04.api product

moje szkolenia - nie wiem na czym sie skupic


jesli {proxy+} jak wykluczyc komendy

jesli ta sama domena dla IP czy mozna zrobic kolejny API tych samych sciezek, w jaki sposob dodawac domene.

gru powanie key-value

secret manager key is wpisywanyh czy generowany i czy mozna genrowac


```
secret_value=$(openssl rand -base64 16)
aws secretsmanager create-secret --name MySecret --secret-string "{\"my_secret_key\": \"$secret_value\"}"





###

aws ssm get-parameters-by-path --path "/users/" --recursive

###

export ID="YOUR_ID"
export PASSWORD="YOUR_PASSWORD"
export API_KEY="YOUR_API_KEY"

curl -H "ID: $ID" -H "Password: $PASSWORD" -H "API_KEY: $API_KEY" https://q8s7yrmd3k.execute-api.us-east-1.amazonaws.com/dev



```
vid lambda auth
https://www.youtube.com/watch?v=al5I9v5Y-kA&t=286s

url/dev
url/dev/a
url/dev/b
url/dev/b/error
url/dev/b/c exist