

[[cloud guru]]


****

### how to vpc+2subnets(diff regions) +2vm(diff regions) + connect ssh to them

Odpowiedź
Aby utworzyć nowy zakres IP i połączyć się z maszyną wirtualną w Google Cloud Platform, wykonaj następujące kroki:

1. Utwórz sieć wirtualną i podsieć

```
gcloud compute networks create new-range-test01 --subnet-mode=custom
```

```
gcloud compute networks subnets create netappsubnet --network=my-vpc --stack-type=IPV4_ONLY --range=10.86.0.0/16 --region=us-central1
```
1. Utwórz maszynę wirtualną

```
bash
gcloud compute instances create vm-netapp --machine-type=e2-medium --zone=us-west4-a --network=my-vpc --subnet=netappsubnet
```
1. Połączenie SSH z maszyną wirtualną
Połącz się z maszyną wirtualną za pomocą SSH:
```
bash
gcloud compute ssh vm-netapp --zone=us-central1-a
```
1. Montowanie woluminu NetApp
Zaloguj się na maszynie wirtualnej:

Połącz się z maszyną wirtualną za pomocą SSH.

Montuj wolumin NetApp:

Jeśli używasz protokołu NFS, możesz zamontować wolumin za pomocą polecenia:
```
bash
sudo mount -t nfs -o rw,hard,rsize=65536,wsize=65536,vers=4.1,tcp,sec=sys 10.86.0.100:/vol1 /mnt/netapp
```
Zastąp 10.86.0.100 adresem IP Twojego woluminu NetApp i /vol1 nazwą woluminu.

1. Sprawdź dostęp do woluminu
Sprawdź dostęp do woluminu:

Wykonaj polecenie, aby sprawdzić, czy wolumin jest poprawnie zamontowany:
```
bash
df -h
```
To polecenie wyświetli listę zamontowanych systemów plików.

Zanotuj ścieżkę dostępu:

Zanotuj ścieżkę dostępu do woluminu NetApp, np. /mnt/netapp.

***
***
```
#
project : playground-s-11-11bd9b09

#
my-vpc

# create vpc

	gcloud compute networks create my-vpc --subnet-mode custom


	gcloud compute networks subnets list --network=my-vpc


# create subnets

	1.
	gcloud compute networks subnets create netapptest01-us-central1 --network=my-vpc --stack-type=IPV4_ONLY --range=10.2.0.0/16 --region=us-central1


	2.
	gcloud compute networks subnets create netapptest02-europe-west1 --network=my-vpc --stack-type=IPV4_ONLY --range=10.12.0.0/16 --region=europe-west1
	
	gcloud compute networks subnets list

# vm
	gcloud compute instances create vm-netapp-us-central1 \
	--machine-type=n1-standard-1 \
	--zone=us-central1-a \
	--network=my-vpc \
	--subnet=netapptest01-us-central1 \
	--no-address
	
	
	gcloud compute instances create vm-netapp-europe-west1 \
	--machine-type=n1-standard-1 \
	--network=my-vpc \
	--zone=europe-west1-c \
	--subnet=netapptest02-europe-west1 \
	--no-address
	
	gcloud compute instances list
	
# firewall-rules / not tested !!!

	gcloud compute firewall-rules create allow-ingress-from-iap \
  --direction INGRESS \
  --action ALLOW \
  --rules tcp:22 \
  --source-ranges 35.235.240.0/20


# ssh
	gcloud compute ssh vm-netapp-us-central1 --zone=us-central1-a

	gcloud compute ssh vm-netapp-europe-west1 --zone=europe-west1-c
	
***
	
# list all rules
	gcloud compute firewall-rules list
	
# wyświetlić szczegóły instancj
	gcloud compute instances describe vm-netapp-us-central1 --zone=us-central1-a

# Jeśli używasz protokołu NFS, możesz zamontować wolumin za pomocą polecenia:
	sudo mount -t nfs -o rw,hard,rsize=65536,wsize=65536,vers=4.1,tcp,sec=sys 10.86.0.100:/vol1 /mnt/netapp
	
# czy wolumin jest poprawnie zamontowany
	df -h
	

```