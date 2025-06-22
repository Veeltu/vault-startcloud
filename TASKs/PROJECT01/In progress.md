
Proszę pomyśleć nad strukturą, może warto też na tym etapie dodać też terragrunta w celu budowy środowisk.

[] Zaczynamy od modułów, które na początek dostarczą sam projekt, vpc oraz włączą odpowiednie API. 
[] Dodatkowo IAM, uprawnienia oraz foldery.
[] Struktura modułowa i wyniesiona do plików wejściowych, które definiują parametry. 
[] Ma to być dość uniwersalne narzędzie. 
[] Stan można trzymać na git a uruchamiać póki co można lokalnie. 
[] Proszę też pamiętać o wersjach danych modułów, w celu łatwej adopcji w przyszłości.

Celem jest zbudowanie modułowej struktury z wykorzystaniem wejściowych parametrów w oddzielnej strukturze np. json. Proszę przy tej okazji pomyśleć w jaki sposób zarządzać adresacją nowych sieci - czyli ipam w gcp. Chodzi o to, że najpierw definiujemy duże sieci w ipam w projekcie a następnie z nich pobieramy do dodawania nowych sieci. Trzeba to podzielić na regiony. IPAM powinien być w innym prjekcie lub tym samym gdzie vpc - to musi być do wyboru.


https://github.com/iancoralogix/multi-cloud-terragrunt-filesystem
https://atmos.tools/features/
https://github.com/gruntwork-io/terraform-fake-modules

https://github.com/gruntwork-io

