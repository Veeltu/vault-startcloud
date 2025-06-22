
https://github.com/hashicorp/terraform-provider-google/issues/10962

Couldn't recreate the issue on the original terraform config that was provided.

But this [doc](https://cloud.google.com/load-balancing/docs/l7-internal/int-https-lb-tf-examples#with_a_mig_backend) has an invalid configuration.

It has `name` instead of `name_prefix` set. The lifecycle rule `create_before_destroy=true` allows for dynamic change of a used template in the `MIG` without destroying every resource tied to it, but when using it with `name` it will just fail with error `already exists etc.`

When using `name_prefix` it will create a random string of numbers that will be used as the template name and allow the create before destroy action because the 2 resources have unique names.

# chodzi by zmienic w template z name na name_prefix

Nie udało się odtworzyć problemu w oryginalnej konfiguracji terraform, która została dostarczona.Jednak ta dokumentacja ma nieprawidłową konfigurację.Zostało ustawione `name` zamiast `name_prefix`. Reguła cyklu życia `create_before_destroy=true` pozwala na dynamiczną zmianę używanego szablonu w MIG bez niszczenia każdego zasobu z nim powiązanego, ale gdy używa się `name`, po prostu zakończy się błędem „już istnieje” itd.Kiedy używasz `name_prefix`, zostanie utworzony losowy ciąg cyfr, który będzie używany jako nazwa szablonu i pozwoli na działanie „tworzenia przed zniszczeniem”, ponieważ dwa zasoby mają unikalne nazwy.

wytłumacznie:

Informacje zawarte w wynikach wyszukiwania dotyczą ogólnego użycia **meta-argumentu lifecycle** w Terraform, który jest stosowany do zarządzania cyklem życia zasobów. Meta-argument ten można zastosować do dowolnego zasobu w Terraform, aby kontrolować, jak te zasoby są tworzone, aktualizowane i niszczone. Oto kluczowe elementy:

## Kluczowe Meta-Argumenty Lifecycle

1. **create_before_destroy**:
    
    - Umożliwia stworzenie nowego zasobu przed zniszczeniem starego, co minimalizuje przestoje. Jest to szczególnie przydatne w przypadku zasobów, które muszą być dostępne przez cały czas, takich jak maszyny wirtualne lub usługi wymagające ciągłej dostępności.
    
2. **prevent_destroy**:
    
    - Zapobiega przypadkowemu usunięciu krytycznych zasobów. Gdy ten argument jest ustawiony na `true`, Terraform zgłosi błąd, jeśli plan wymaga zniszczenia zasobu.
    
3. **ignore_changes**:
    
    - Ignoruje zmiany w określonych atrybutach lub całym obiekcie, co jest przydatne w sytuacjach, gdy zewnętrzne zmiany nie powinny wpływać na stan zarządzany przez Terraform.
    
4. **replace_triggered_by**:
    
    - Umożliwia określenie zależności między zasobami, co może prowadzić do ich wymiany na podstawie zmian w innych zasobach.
    

## Zastosowanie

Meta-argumenty te mogą być stosowane do różnych typów zasobów w Terraform, takich jak:

- Grupy zasobów (np. `azurerm_resource_group`)
- Maszyny wirtualne (np. `azurerm_virtual_machine`)
- Bazy danych (np. `azurerm_sql_database`)

## Podsumowanie

Informacje zawarte w wynikach dotyczą ogólnych zasad działania meta-argumentu lifecycle w Terraform i mogą być stosowane do różnych typów zasobów w zależności od potrzeb użytkownika oraz specyfiki infrastruktury. Użycie tych argumentów pozwala na lepsze zarządzanie cyklem życia zasobów oraz minimalizację ryzyka podczas operacji związanych z infrastrukturą.