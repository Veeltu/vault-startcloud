locals
Używa się ich do przechowywania wartości, które są obliczane na podstawie innych zmiennych lub atrybutów zasobów i będą używane wielokrotnie w konfiguracji.
Przykład:
text
locals {
  instance_name = "${var.environment}-instance"
}

variables
Używa się ich do definiowania parametrów, które można konfigurować przy uruchamianiu Terraformu. Mogą mieć domyślne wartości oraz opisy.
Przykład:
text
variable "environment" {
  description = "The environment to deploy to"
  type        = string
  default     = "development"
}

3. Kiedy używać locals, a kiedy variables?
Używaj locals, gdy:
Potrzebujesz wartości, które są obliczane na podstawie innych zmiennych lub atrybutów zasobów.
Chcesz uniknąć duplikacji kodu w ramach jednego modułu.
Wartość nie musi być przekazywana poza dany moduł.
Używaj variables, gdy:
Chcesz umożliwić użytkownikom konfigurowanie wartości przy uruchamianiu Terraformu.
Potrzebujesz wartości, które będą przekazywane do podmodułów lub będą miały domyślne wartości.