Oto wyjaśnienie składni polecenia:

```bash
terragrunt import kubernetes_namespace.network network
```
kubernetes_namespace.network – adres zasobu w kodzie
network – nazwa istniejącego namespace w klastrze

```
terragrunt import <resource_address> <id>
```



### Składnia i znaczenie poszczególnych elementów

- **terragrunt import**  
  To polecenie uruchamia funkcję importu w Terragrunt, która jest wrapperem na komendę `terraform import`. Pozwala dodać istniejący zasób (np. namespace w Kubernetes) do stanu Terraform, aby był zarządzany przez IaC[1][2][11].

- **kubernetes_namespace.network**  
  To tzw. *address* (adres zasobu) – wskazuje na konkretny blok zasobu w Twojej konfiguracji Terraform.  
  - `kubernetes_namespace` – typ zasobu, którym zarządza provider Kubernetes w Terraformie.
  - `network` – nazwa logiczna (label) tego zasobu w pliku konfiguracyjnym Terraform, np. w bloku:
    ```hcl
    resource "kubernetes_namespace" "network" {
      metadata {
        name = "network"
      }
    }
    ```
  Dzięki temu Terraform wie, do którego bloku w kodzie przypisać importowany zasób[1][2].

- **network**  
  To *id* (identyfikator) zasobu w zewnętrznym systemie – w tym przypadku jest to nazwa namespace w klastrze Kubernetes. Importujesz istniejący namespace o nazwie `network` do bloku `kubernetes_namespace.network` w swoim stanie Terraform[1][2].

---

### Podsumowanie

| Składnik polecenia                  | Znaczenie                                                                                   |
|--------------------------------------|--------------------------------------------------------------------------------------------|
| terragrunt import                    | Rozpocznij import istniejącego zasobu do stanu Terraform przez Terragrunt                  |
| kubernetes_namespace.network         | Adres zasobu w konfiguracji Terraform (typ + nazwa logiczna)                               |
| network                             | Identyfikator zasobu w zewnętrznym systemie (tu: nazwa namespace w Kubernetes)             |

Import pozwala Ci przejąć zarządzanie istniejącym zasobem przez Terraform/Terragrunt, bez konieczności jego ponownego tworzenia[1][2][11].

[1] https://terrateam.io/blog/terraform-import
[2] https://spacelift.io/blog/importing-exisiting-infrastructure-into-terraform
[3] https://registry.terraform.io/providers/hashicorp/kubernetes/latest/docs/resources/namespace
[4] https://github.com/hashicorp/terraform-provider-kubernetes/issues/2146
[5] https://terragrunt.gruntwork.io/docs/reference/config-blocks-and-attributes/
[6] https://github.com/hashicorp/terraform-provider-kubernetes/issues/980
[7] https://registry.terraform.io/providers/hashicorp/kubernetes/2.28.0/docs/resources/namespace.html
[8] https://spacelift.io/blog/terraform-kubernetes-provider
[9] https://github.com/fluxraum/terragrunt-import-demo
[10] https://blog.shellnetsecurity.com/2023/10/651/github/terraform-import-made-easy-conquering-infrastructure-management-complexities/
[11] programming.terraform