## Kiedy używać `dynamic`, a kiedy `for_each` w Terraform?

W Terraform zarówno `dynamic`, jak i `for_each` są używane do iteracji, ale mają różne zastosowania i konteksty. Oto kluczowe różnice i wskazówki dotyczące ich użycia:

## `for_each`

- **Definicja**: `for_each` jest meta-argumentem, który można zastosować do zasobów i modułów w celu utworzenia wielu instancji na podstawie kolekcji (mapy lub zbioru).
- **Zastosowanie**:
    
    - Używaj `for_each`, gdy chcesz utworzyć wiele niezależnych zasobów na podstawie listy lub mapy.
    - Każdy zasób utworzony przy użyciu `for_each` ma dostęp do swojego klucza (jeśli jest to mapa), co ułatwia zarządzanie nimi.
    
- **Przykład**:text
    
    `resource "aws_instance" "example" {   for_each = var.instance_ids  # Lista ID instancji   ami           = var.ami_id  instance_type = var.instance_type }`
    

## `dynamic`

- **Definicja**: Dynamic block działa podobnie do wyrażenia `for`, ale generuje zagnieżdżone bloki zamiast wartości. Umożliwia tworzenie powtarzalnych zagnieżdżonych bloków w obrębie zasobów.
- **Zastosowanie**:
    
    - Używaj `dynamic`, gdy potrzebujesz wygenerować wiele zagnieżdżonych bloków w obrębie jednego zasobu na podstawie kolekcji.
    - Jest to przydatne, gdy masz wiele podobnych konfiguracji w ramach jednego zasobu, na przykład reguły zapory ogniowej lub ustawienia środowiska.
    
- **Przykład**:text
    
    `resource "aws_security_group" "example" {   name        = "example_sg"  description = "Example security group"   dynamic "ingress" {    for_each = var.ingress_rules  # Lista reguł dostępu     content {      from_port   = ingress.value.from_port      to_port     = ingress.value.to_port      protocol    = ingress.value.protocol      cidr_blocks = ingress.value.cidr_blocks    }  } }`
    

## Podsumowanie

- **Użyj `for_each`**, gdy chcesz tworzyć wiele niezależnych instancji zasobów na podstawie kolekcji, co pozwala na łatwe zarządzanie nimi za pomocą kluczy.
- **Użyj `dynamic`**, gdy musisz generować powtarzalne zagnieżdżone bloki w obrębie jednego zasobu, co pozwala na uniknięcie duplikacji kodu i zwiększa czytelność konfiguracji.

Dzięki tym wskazówkom możesz skutecznie korzystać z obu funkcji w Terraform, aby zoptymalizować swoje skrypty infrastruktury jako kodu (IaC).
