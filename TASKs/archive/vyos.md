
from Radek:
```
ssh vyos@IP
ssh vyos@100.100﻿.81.2


set terminal width 1000  

sh configuration

sh vrf - jakie sie vrf-y i do czego przpisane
  
show bgp vrf all summary  
show ip route vrf RTN  
show ip bgp vrf customer neighbors 100.101.48.0 advertised-routes

z prezentacji:

show configuration commands
show ip route
show ip route vrf RTN
show bgp vrf all summary


# co jest wysylane do routera
sh ip bgp vrf RTN neighbors 'IP' advertised-routes
sh ip bgp vrf RTN neighbors 100.100.81.20 advertised-routes - example 



```

1. login to VM-bastrion  in project- **nz-mgmt-transfer-svpc**
ssh vyos@IP
ssh vyos@100.100﻿.81.2

if wont work, go there in GCP/browser

2. password -> secret manager -> click on -> show secret


RTN - wewnetrzy w GCP, np jak połączenia routera z routerem
ETN - na zewątrz GCP

VRF - **VRF (Virtual Routing and Forwarding)** is a technology that allows multiple instances of a routing table to coexist within the same router or switch. This capability enables network operators to segment traffic without requiring multiple physical devices, effectively creating isolated virtual networks.

neighbors - Sąsiad to inny router, z którym nawiązano połączenie BGP.

VPN - 

BGP - Border Gateway Protocol

RR - route reflector

to co otrzymuje i to co rozglaszam

redundacja

maski sieciowe - Maski sieciowe, czyli maski podsieci, to 32-bitowe liczby, które dzielą adres IP na część sieciową i część hosta. Umożliwiają one routerom identyfikację, które bity adresu IP należą do sieci, a które do konkretnego urządzenia w tej sieci.

21 min start co robic

task story : 
posprawdac dokad co idzie skupiajac sie na czesci wew - 10.89.x.x odrzucamy

 sprawdzic dokąd to idzie i na co to wskazuje ( directed conneced or not), odniesienie to tabeli z pdf 
żeby było wygodnie sprwdzic dokad to idzie za pomoca:

```
show bgp vrf all summary

# wyświetla podsumowanie stanu protokołu BGP (Border Gateway Protocol) dla wszystkich instancji VRF (Virtual Routing and Forwarding) skonfigurowanych na routerze.


show ip route vrf RTN 
show ip route vrf customer

# wyświetla informacje o trasach routingu skonfigurowanych w instancji VRF (Virtual Routing and Forwarding) o nazwie "RTN". czyli - JAKIE TRASY SA DOSTEPNE W TABELI

sh ip bgp vrf customer neighbors @IP@ advertised-routes

# służy do wyświetlania tras, które są ogłaszane przez konkretnego sąsiada BGP dla instancji VRF (Virtual Routing and Forwarding) o nazwie "customer". CZYLI JAKIE TRASY SA REKLAMOWANE, OGLASZANE


```
  nowa tabela 
inny kolor jezeli chodzi o BGP
znak zapytania na lini z errorem

co jest rozglasdzane z ktorej strony

0000 oznacza ze next hopem jestesmy sami, pierwszy adres z sieci


tylko transfer svpc


radoslaw.starczewski@management.nzero.cloud