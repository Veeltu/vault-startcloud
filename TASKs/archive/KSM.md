Proszę o przygotowanie jakie są wymaganie odnośnie routingu i rulek FW w celu dostępu do KSM w GCP z on-prem ([![](https://www.gstatic.com/devrel-devsite/prod/v870e399c64f7c43c99a3043db4b3a74327bb93d0914e84a0c3dba90bbfd67625/cloud/images/favicons/onecloud/favicon.ico)Cloud Key Management Service documentation  |  Cloud KMS Documentation  |  Google Cloud](http://cloud.google.com/kms/docs) ).

Zapytanie odnosi się do zrealizowania dostępu w środowisku GCP, dla którego przygotowuje Pan dokumentację. Możliwe, że będzie to dotyczyło organizacji MGT jak i workload (dev, test, prod).

Proszę rozeznać temat i zebrać wymagania od strony technicznej.
***

jakie są wymagania - **routing** i **rules from FW** w celu dostępu do KSM w GCP z on-prem.
czyli : z on-prem chcemy miec dostep do KSM.
jak zrealizowac dostep do KSM ?

***
Cloud External Key Manager ???

how to get access to KMS from on-prem ?

***

### what is KMS - 
Umożliwia bezpieczne przechowywanie kluczy szyfrujących, które są wykorzystywane do ochrony danych w różnych usługach Google Cloud[

## Cloud KMS ma wiele zastosowań, w tym:

- **Szyfrowanie danych w spoczynku:** Umożliwia szyfrowanie danych przechowywanych w usługach takich jak Cloud Storage i Cloud SQL.
- **Szyfrowanie danych w ruchu:** Może być zintegrowane z innymi usługami Google Cloud, aby zapewnić bezpieczeństwo danych przesyłanych przez sieć.
- **Zarządzanie kluczami aplikacji:** Umożliwia tworzenie i zarządzanie kluczami szyfrującymi używanymi przez aplikacje.
- **Zgodność z przepisami:** Pomaga spełniać wymagania dotyczące zgodności z regulacjami, takimi jak PCI DSS i HIPAA[

## Funkcje bezpieczeństwa

Cloud KMS oferuje szereg funkcji bezpieczeństwa, takich jak:

- **Moduły bezpieczeństwa sprzętowego (HSM):** Z certyfikatem FIPS 140-2 Level 3.
- **Szczegółowe dzienniki audytu:** Umożliwiają monitorowanie operacji związanych z kluczami.
- **Integracja z Identity and Access Management (IAM):** Umożliwia kontrolowanie dostępu do kluczy