#netapp


proszę dodać 2 sieci PSA,
pozniej niech Pan doda jeden netapp w jednym regione np uscentral 
i podać jedną z sieci PSA - sieć #1 ,
a nastepnie doać kolejny netapp w innym regionie i podac sieć #2 - pytanie czy 2 netapp mimio podanej sieci uzyje je czy raczej bedzie przypisywal siec numer 1

proszę dodać 2 sieci PSA 
doda jeden netapp w jednym regione np uscentral 
i podać jedną z sieci PSA - sieć #1 

a nastepnie doać kolejny netapp w innym regionie i podac sieć #2 -

pytanie czy 2 netapp mimio podanej sieci uzyje je czy raczej bedzie przypisywal siec numer 1

----

"i podać jedną z sieci PSA - sieć #1 ," - podczas tworzenia "netapp storage-pool" nie definujemy z jakies sieci PSA bedzie korzystac ( z dokumentacji wynika ze jest to nie aktywne), mozemy tylko wskazac lokalizacje i vpc.

czyli mozemy ustawic tylko netapp do danego vpc, w okreslonym IP range PSA (moze byc kilka)

z dokumentacji wynika ze :
- Reserve a static internal IP address range for your CIDR:
```
gcloud compute addresses create netapp-addresses-production-vpc1 \
 --project=my-project \
 --global \
 --purpose=VPC_PEERING \
 --prefix-length=24 \
 --network=VPC \
 --no-user-output-enabled
  --addresses=192.168.0.0 \
 ```
- peer the networks
```
  gcloud services vpc-peerings connect \
     --project=my-project \
   --service=netapp.servicenetworking.goog \
   --ranges=netapp-addresses-production-vpc1<,psarange2> \
   --network=VPC
```
- Enable custom route propagation. Note that NetApp Volumes creates a sn-netapp-prod peering when the private connection is set up.
```
  gcloud compute networks peerings update sn-netapp-prod \
     --project=my-project \
    --network=VPC  \
    --import-custom-routes \
    --export-custom-routes
```

- create a storage pool 
```
    gcloud netapp storage-pools create NAME \
       --project=my-vpc \
       --location=LOCATION \
       --capacity=CAPACITY \
       --service-level=SERVICE_LEVEL \
       --network=name=NETWORK_NAME,psa-range=PSA_RANGE
```
here psa-range-PSA_RANGE is not supported
[img]

we specify only LOCATION and VPC.

new-range-test01
10.2.0.0/24
netapp.servicenetworking.goog
sn-netapp-prod


psarange2
10.86.0.0/16
netapp.servicenetworking.goog
sn-netapp-prod

test-service-directory