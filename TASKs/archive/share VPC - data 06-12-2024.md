data: 06-12-2024

### task:
Proszę o przygotowanie kodu w TF na tworzenie Shared VPC z sieciami do wyboru (w postaci struktury) oraz dodawania projektów dynamicznie do Shared VPC - również dynamicznie. 

Proszę sprawdzić jakie uprawnienia są potrzebne, żeby projekty serwisowe miały dostęp do poszczególnych sieci z VPC - proszę sprawdzić jakie są wymogi od strony uprawnień. 
Uprawnienia należy również dodać w kodzie - dynamicznie - per projekt w korelacji z siecią.

***
Shared VPC
1. tworzenie Shared VPC z sieciami do wyboru
2. dynamiczne dodawanie projektow do SharedVPC (z dopiskami, bez wymyslania nazw)
3. sprawdzic uprawnienia zeby projekty mialy dostep do "poszczegolnych sieci VPC"
4. uprawnienia w kodzie,dynamicznie, per projekt w korelacji z siecia

***


https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/compute_shared_vpc_host_project

```
# A host project provides network resources to associated service projects.
resource "google_compute_shared_vpc_host_project" "host" {
  project = "host-project-id"
}

# A service project gains access to network resources provided by its
# associated host project.
resource "google_compute_shared_vpc_service_project" "service1" {
  host_project    = google_compute_shared_vpc_host_project.host.project
  service_project = "service-project-id-1"
}

resource "google_compute_shared_vpc_service_project" "service2" {
  host_project    = google_compute_shared_vpc_host_project.host.project
  service_project = "service-project-id-2"
}
```

https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/compute_shared_vpc_service_project

https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/google_project

***
# ?? lista servis_projects ? 

 1. set host-sharedVPC in project

 2. get list of subnets in VPC

 3. filter subnet by prefix

 4. add filtered subnets to servise_projects


[x ]list of subnets 
[x] crate host
[x] attach service_projects existed / how to get a list ? to exsited not attached projects ?


w zaleznosci jaki subnet dajemy uprawnienia do danego projektu         copute nerwork admin

i on tylko po tym sufixie widzie jakie podsieci dodac do projektu
if XXX add to Project1 ?

host :
_Shared VPC Admin role_ 
Compute Network Admin role
serivce:
_Compute Network User role_

***

tak nie dziala...

ale gydby tak zrobic taka liste :

```
105867941633-compute@developer.gserviceaccount.com : "sub-aaa",
105867941633-compute@developer.gserviceaccount.com : "su2b-aaa",

105867941633-compute@developer.gserviceaccount.com : "sub-bbb",
105867941633-compute@developer.gserviceaccount.com : "sub2-bbb",

etc
```

***

Przypomnę się odnośnie w temacie terraforma dla sharedVPC.
Mam jedynie pytanie, czy aby na pewno danie dostępu danego service-project do danego subnetu w host-project, odbywa się przez nadanie policy ( subnetowi w host-project ) z wpisaniem "member" dla danego service-project ?  