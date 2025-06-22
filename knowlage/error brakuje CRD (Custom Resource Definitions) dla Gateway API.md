Błąd wskazuje, że brakuje CRD (Custom Resource Definitions) dla Gateway API w twoim klastrze MicroK8s. Oto konkretne rozwiązania:

 Error: API did not recognize GroupVersionKind from manifest (CRD may not be installed)

## 1. Aktywuj Gateway API w MicroK8s
MicroK8s ma wbudowane wsparcie dla Gateway API przez addon `gateway`:
```bash
microk8s enable gateway
```
Sprawdź status:
```bash
microk8s status --wait-ready | grep gateway
```
Powinno pokazać `gateway: enabled`.

## 2. Ręczna instalacja CRD (jeśli addon nie działa)
Jeśli addon nie działa, zainstaluj CRD ręcznie:
```bash
kubectl apply -f https://github.com/kubernetes-sigs/gateway-api/releases/latest/download/standard-install.yaml
kubectl apply -f https://github.com/kubernetes-sigs/gateway-api/releases/latest/download/experimental-install.yaml
```

## 3. Konfiguracja Terraform
W konfiguracji Terraform upewnij się, że:
- Używasz `depends_on` dla zależności od CRD
- Konfigurujesz provider Kubernetes z jawnymi credentialami

Przykład providera:
```hcl
provider "kubernetes" {
  host                   = "https://127.0.0.1:16443"
  cluster_ca_certificate = file("~/.kube/certs/ca.crt")
  client_certificate     = file("~/.kube/certs/client.crt")
  client_key             = file("~/.kube/certs/client.key")
}
```

## 4. Sekwencja wdrażania
Wykonaj wdrażanie w odpowiedniej kolejności:
```bash
# 1. Zainstaluj CRD
terragrunt apply -target=module.gateway_crds

# 2. Wdróż resztę infrastruktury
terragrunt apply
```

## 5. Sprawdź zainstalowane CRD
Potwierdź, że CRD są dostępne:
```bash
kubectl get crd gateways.gateway.networking.k8s.io
kubectl get crd tcproutes.gateway.networking.k8s.io
```

## Rozwiązywanie typowych problemów
- **Błąd `no matches for kind "Gateway"`**: CRD nie są zainstalowane lub Terraform nie widzi zmian (wykonaj `terraform refresh`)
- **Błąd autoryzacji**: Sprawdź certyfikaty w kubeconfig MicroK8s (`microk8s config > ~/.kube/config`)
- **Niezgodność wersji API**: Upewnij się, że w manifestach używasz `gateway.networking.k8s.io/v1beta1` dla TCPRoute

Jeśli problem pozostaje, podaj output z:
```bash
kubectl get crd -A
kubectl api-versions | grep gateway
```

[1] https://stackoverflow.com/questions/69461513/no-matches-for-kindgateway-and-virtualservice
[2] https://stackoverflow.com/questions/76672533/i-am-trying-to-apply-a-yaml-file-into-my-cluster-but-getting-error-ensure-crds
[3] https://github.com/hashicorp/terraform-provider-kubernetes/issues/2597
[4] https://gateway-api.sigs.k8s.io/guides/
[5] https://blog.devops.dev/securing-kubernetes-microservices-with-istio-mtls-gateways-and-the-kubernetes-gateway-api-c1112a782edc
[6] https://stackoverflow.com/questions/79276863/terraform-deployment-failure-on-constructing-rest-client
[7] https://github.com/hashicorp/terraform-provider-kubernetes/issues/2474
[8] https://devtron.ai/blog/kubernetes-gateway-api/
[9] https://stackoverflow.com/questions/77119996/how-to-make-terraform-ignore-a-resource-if-another-one-is-not-deployed
[10] https://cloud.google.com/kubernetes-engine/docs/how-to/deploying-gateways
[11] https://github.com/istio/istio/issues/23894
[12] https://stackoverflow.com/questions/69054622/unable-to-install-crds-in-kubernetes-kind
[13] https://kubernetes.io/docs/concepts/services-networking/gateway/
[14] https://istio.io/latest/docs/tasks/traffic-management/ingress/gateway-api/
[15] https://registry.terraform.io/providers/hashicorp/kubernetes/latest/docs/resources/manifest
[16] https://github.com/hashicorp/terraform-provider-kubernetes/issues/2739
[17] https://registry.terraform.io/providers/hashicorp/kubernetes/latest/docs
[18] https://developer.hashicorp.com/consul/docs/upgrade/api-gateway
[19] https://gateway-api.sigs.k8s.io/reference/spec/
[20] https://github.com/cilium/cilium/issues/34540
[21] https://clouddocs.f5.com/bigip-next-for-kubernetes/2.0.0-LA/gateway-api-crd.html
[22] https://gateway-api.sigs.k8s.io/guides/implementers/
[23] https://doc.traefik.io/traefik/providers/kubernetes-gateway/
[24] https://kgateway.dev/docs/operations/install/
[25] https://kgateway.dev/docs/quickstart/
[26] https://docs.cilium.io/en/latest/network/servicemesh/gateway-api/gateway-api.html
[27] https://gateway-api.sigs.k8s.io
[28] https://developer.hashicorp.com/terraform/tutorials/kubernetes/kubernetes-crd-faas
[29] https://kubernetes.io/blog/2020/06/working-with-terraform-and-kubernetes/
[30] tools.kubernetes
[31] programming.terraform