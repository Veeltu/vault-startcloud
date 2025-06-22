
Komenda:

```bash
microk8s config > ~/.kube/config
```

**generuje plik konfiguracyjny kubeconfig dla Twojego klastra MicroK8s i zapisuje go jako domyślny plik konfiguracyjny Kubernetes w katalogu domowym użytkownika (`~/.kube/config`)**[1].

Dzięki temu:
- narzędzia takie jak `kubectl`, Terraform, Terragrunt oraz inne aplikacje korzystające z Kubernetes automatycznie używają poprawnych danych dostępowych (adres API, certyfikaty, użytkownik) do Twojego klastra MicroK8s,
- nie musisz ręcznie edytować pliku konfiguracyjnego ani wskazywać ścieżki do kubeconfig przy każdym poleceniu.

Jest to standardowy sposób na skonfigurowanie środowiska pracy z Kubernetes, szczególnie po reinstalacji lub zmianie ustawień klastra[1].

[1] tools.kubernetes

***
error after apply

```
06:38:16.196 STDERR terraform: │ Error: Deployment exceeded its progress deadline
06:38:16.196 STDERR terraform: │
06:38:16.197 STDERR terraform: │ with kubernetes_deployment_v1.collector,
06:38:16.197 STDERR terraform: │ on main.tf line 113, in resource "kubernetes_deployment_v1" "collector":
06:38:16.197 STDERR terraform: │ 113: resource "kubernetes_deployment_v1" "collector" {
06:38:16.197 STDERR terraform: │
06:38:16.197 STDERR terraform: ╵
06:38:16.250 ERROR terraform invocation failed in ./.terragrunt-cache/Ag3F5FhNI_w9IY2xZWvVivXYFlU/TUKEo4OkF46_ABmOdUIKIl0SvwY
06:38:16.251 ERROR error occurred:

* Failed to execute "terraform apply" in ./.terragrunt-cache/Ag3F5FhNI_w9IY2xZWvVivXYFlU/TUKEo4OkF46_ABmOdUIKIl0SvwY
╷
│ Error: Deployment exceeded its progress deadline
│
│ with kubernetes_deployment_v1.collector,
│ on main.tf line 113, in resource "kubernetes_deployment_v1" "collector":
│ 113: resource "kubernetes_deployment_v1" "collector" {
│
╵

exit status 1

veeltu@host-veeltu ~/o/c/d/otel-collector (main) [1]> ls
collector-agent.yaml Makefile role.yaml terragrunt.hcl
veeltu@host-veeltu ~/o/c/d/otel-collector (main)> sudo microk8s kubectl get pods -n network
[sudo] password for veeltu:
NAME READY STATUS RESTARTS AGE
otel-collector-6d545648f4-jd94l 0/1 ContainerCreating 0 2m26s
prom-snmp-exporter-c698786fc-trvs5 1/1 Running 0 11m
veeltu@host-veeltu ~/o/c/d/otel-collector (main)> sudo microk8s kubectl logs -n network otel-collector-6d545
648f4-jd94l
Error from server: Get "https://10.0.2.15:10250/containerLogs/network/otel-collector-6d545648f4-jd94l/otel-collector": tls: failed to verify certificate: x509: certificate is valid for 10.30.10.6, 192.168.0.115, 2a02:a31d:e0bd:e80:1b64:75fc:1979:2573, 2a02:a31d:e0bd:e80:f947:3c79:8988:5078, not 10.0.2.15
```

```
sudo microk8s.refresh-certs --cert=server.crt
sudo microk8s.refresh-certs --cert=front-proxy-client.crt
sudo microk8s stop
sudo microk8s start
```

