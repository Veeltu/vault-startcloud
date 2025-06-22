
https://cloud.google.com/service-directory/docs
https://cloud.google.com/dns


Wracając jeszcze do Service Directory

Service Directory to usługa rejestru usług dla mikrousług wewątrz gcp.
Czyli jak tworzymy endpoint w gcp to od razu powstaje Service Directory DNS zone, który później jest wykorzystywany do komunikacji pomiędzy mikro-usługami. 

Główny problem jaki to ma rozwiązywać to sytuacja kiedy zmieniają się dynamicznie zasoby i ich adresy + żeby mieć wszystkie rozproszone usługi w jednym miejscu i mieć łatwo do nich dostęp.

Tutaj link : 


// ale dla czego tworzy u klienta sd dns zone 


Imagine that you are building a simple API and that your code needs to call some other application. When endpoint information remains static, you can hard-code these locations into your code or store them in a small configuration file. However, with microservices and multi-cloud , this problem becomes much harder to solve as instances, services, and environments can all change.

**With Service Directory, you can register all of your services in a single place and resolve them by using HTTP, gRPC, and DNS.**

Each service instance is registered with Service Directory. These registrations are immediately reflected in DNS and can be queried by using HTTP/gRPC regardless of their implementation and environment.

You can create a universal service name that works across Google Cloud products, like App Engine and GKE. You can make these services available over DNS. You can apply access controls to services based on network, project, and IAM roles of service accounts.

https://cloud.google.com/service-directory/docs/overview

Wyobraź sobie, że budujesz prosty interfejs API i że twój kod musi wywołać inną aplikację. Gdy informacje o punktach końcowych pozostają statyczne, możesz na sztywno zakodować te lokalizacje w swoim kodzie lub przechować je w małym pliku konfiguracyjnym. Jednak w przypadku mikrousług i multi-cloud problem ten staje się znacznie trudniejszy do rozwiązania, ponieważ instancje, usługi i środowiska mogą się zmieniać.

**Service Directory pozwala zarejestrować wszystkie swoje usługi w jednym miejscu i rozwiązywać je za pomocą HTTP, gRPC i DNS.**

Każda instancja usługi jest rejestrowana w Service Directory. Te rejestracje są natychmiast odzwierciedlane w DNS i mogą być odpytywane za pomocą HTTP/gRPC, niezależnie od ich implementacji i środowiska.

Możesz stworzyć uniwersalną nazwę usługi, która działa w produktach Google Cloud, takich jak App Engine i GKE. Możesz udostępnić te usługi przez DNS. Możesz zastosować kontrole dostępu do usług na podstawie sieci, projektu i ról IAM kont usługowych.





# Co to jest Service Directory?

Service Directory to w pełni zarządzana, wysoce dostępna usługa rejestru usług dla mikrousług. Umożliwia ona bezpieczne rejestrowanie wystąpień usług i wyszukiwanie ich przy użyciu nazw usług. Service Directory pomaga w zarządzaniu i upraszczaniu mikrousług w środowiskach rozproszonych, zapewniając centralne repozytorium do przechowywania i odnajdywania informacji o usługach. Eliminuje to potrzebę ręcznego konfigurowania i konserwacji systemów wykrywania usług, upraszczając komunikację i wykrywanie między mikrousługami.

## Do czego służy Service Directory?

Service Directory ułatwia zarządzanie i łączenie mikrousług w środowiskach rozproszonych. Oto kilka przykładów zastosowań Service Directory:

- Wykrywanie usług: Service Directory działa jako centralne repozytorium, w którym mikrousługi mogą rejestrować swoje wystąpienia i odnajdywać inne usługi.
- Zarządzanie usługami: Service Directory zapewnia scentralizowane miejsce do zarządzania metadanymi usług, takimi jak punkty końcowe, wersje i inne informacje konfiguracyjne.
- Uproszczenie sieci: Service Directory upraszcza komunikację między mikrousługami, eliminując potrzebę złożonych mechanizmów routingu.

## Ile kosztuje Service Directory?

Ceny Service Directory zależą od ilości operacji odczytu i zapisu do rejestru usług, a także od ilości przechowywanych danych. Szczegółowe informacje na temat cennika można znaleźć na stronie internetowej Google Cloud Platform.

https://bigglo.pl/google-cloud/service-directory/
***


If we look at the home page for Service Directory: [https://cloud.google.com/service-directory](https://cloud.google.com/service-directory) and compare that to the the home page for Cloud DNS: [https://cloud.google.com/dns](https://cloud.google.com/dns) we start to see the significant distinction between the two.  I'd encourage you to review the two links above.  
At the highest level, service directory is a Google service where services that you manage/own can be registered for lookup.  Think of a service as an application or API capable of doing something for your solutions or business.  For example, if we look at this current web-site, it may have an internal service for "member lookup" which could be called by the web page to find the icon for my profile picture.  This would be an example of a service that could be registered in service directory.  It may be given a logical name such as "PictureLookup".  The service may have meta data such as who owns it, what versions are available, what APIs does it expose and many more.  The value of service directory is to allow programmers (and applications) to lookup services for use.   When a service is identified for use, it will likely have a location where it exists that should be used to invoke it.  This might be [http://profile-picture-lookup.prod.mycompany.com](http://profile-picture-lookup.prod.mycompany.com/) .... when computers need to connect to a service, they need the IP address of that service.  Trying to remember and work with IP addresses is not sensible because we can't remember that 1.2.3.4 is server A and 1.2.3.5 is server B.  Instead, we need a catalog that allows us to map from "human" consumable names to the IP addresses.  This is where DNS comes into play.  DNS allows us to map server names to IP addresses.  Google's Cloud DNS provides the management of mapping of names to IP addresses that you own.  For example, if you own the domain name "mycompany.com", if you expose a machine with IP address 1.2.3.4 and want that to be "known" as "myserver", then you would use a product such as Cloud DNS to own the mapping to myserver.mycompany.com

https://www.googlecloudcommunity.com/gc/Infrastructure-Compute-Storage/Difference-between-Cloud-DNS-and-Service-Directory/m-p/162260
***
#### translation to PL
 Na najwyższym poziomie Service Directory to usługa Google, w której usługi, którymi zarządzasz/które posiadasz, mogą być rejestrowane w celu wyszukiwania. 
 Pomyśl o usłudze jako o aplikacji lub interfejsie API zdolnym do robienia czegoś dla twoich rozwiązań lub firmy. Na przykład, jeśli spojrzymy na tę obecną witrynę internetową, może ona mieć wewnętrzną usługę „wyszukiwania członków”, która mogłaby być wywoływana przez stronę internetową w celu znalezienia ikony dla mojego zdjęcia profilowego. To byłby przykład usługi, którą można zarejestrować w Service Directory. 
 Można jej nadać logiczną nazwę, taką jak „PictureLookup”. Usługa może mieć metadane, takie jak informacje o właścicielu, dostępne wersje, udostępniane interfejsy API i wiele innych. Wartość Service Directory polega na umożliwieniu programistom (i aplikacjom) wyszukiwania usług do wykorzystania. Kiedy usługa zostanie zidentyfikowana do użycia, prawdopodobnie będzie miała lokalizację, w której się znajduje, która powinna być używana do jej wywołania. To może być [http://profile-picture-lookup.prod.mycompany.com](http://profile-picture-lookup.prod.mycompany.com/) 
 .... 
 kiedy komputery muszą łączyć się z usługą, potrzebują adresu IP tej usługi. Próba zapamiętywania i pracy z adresami IP nie jest rozsądna, ponieważ nie możemy zapamiętać, że 1.2.3.4 to serwer A, a 1.2.3.5 to serwer B. Zamiast tego potrzebujemy katalogu, który pozwoli nam mapować nazwy zrozumiałe dla „ludzi” na adresy IP. I tu do gry wchodzi DNS. 
 DNS pozwala nam mapować nazwy serwerów na adresy IP. Cloud DNS Google zapewnia zarządzanie mapowaniem nazw na adresy IP, których jesteś właścicielem. Na przykład, jeśli jesteś właścicielem nazwy domeny „mycompany.com”, jeśli udostępniasz maszynę o adresie IP 1.2.3.4 i chcesz, aby była „znana” jako „myserver”, użyjesz produktu takiego jak Cloud DNS, aby posiadać mapowanie na myserver.mycompany.com

