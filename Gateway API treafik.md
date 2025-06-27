Poniższe podsumowanie przedstawia, jak ruch przepływa przez Gateway API w Twoim klastrze Kubernetes, ze wskazaniem kluczowych fragmentów konfiguracji Terraform, które to implementują.

## 1. Rozwiązywanie DNS i ruch do Service Traefika
Ruch zewnętrzny do domeny takiej jak gw.observability.test.pndrs.de najpierw musi zostać przetłumaczony na adres IP.

#### Zarządzanie DNS za pomocą ExternalDNS:
Opis: Adnotacja na Service Traefika instruuje kontroler ExternalDNS, aby automatycznie utworzył rekord DNS wskazujący na zewnętrzny adres IP LoadBalancera Service Traefika.

Kod: Jest to zdefiniowane w konfiguracji Helm dla Traefika:
```
resource "helm_release" "traefik" {
  # ...
  values = [
    yamlencode({
      # ...
      service = {
        annotations = {
          "external-dns.alpha.kubernetes.io/hostname" = "gw.observability.test.pndrs.de"
        }
```

#### Wystawianie Traefika przez Service Kubernetes:
Opis: Kubernetes Service dla Traefika wystawia go na zewnątrz klastra, udostępniając porty do nasłuchu.

Kod: Porty są konfigurowane w helm_release.traefik:
```
resource "helm_release" "traefik" {
  # ...
  values = [
    yamlencode({
      # ...
      ports = merge({
        for key, val in local.tcpRoutes : key => {
          name        = key
          port        = val.port
          exposedPort = val.port
          protocol    = "TCP"
          expose = {
            default = true

```
Oprócz tego, porty 8000 (HTTP) i 8443 (HTTPS) są używane przez Gatewaya.

## 2. Traefik jako Kontroler Gatewaya
Traefik jest wdrażany w klastrze i konfigurowany do zarządzania zasobami Gateway API.

#### Wdrożenie Traefika (Deployment):

Opis: Helm Chart wdraża pody Traefika w dedykowanej przestrzeni nazw (traefik).

Kod: Liczba replik jest ustawiona w helm_release.traefik:
```
resource "helm_release" "traefik" {
  # ...
  values = [
    yamlencode({
      # ...
      deployment = {
        replicas = 3 # Trzy repliki zapewniają wysoką dostępność
```

#### Włączenie Providera Kubernetes Gateway:

Opis: Traefik jest skonfigurowany, aby obserwował i zarządzał zasobami Gateway API (Gateway, HTTPRoute, TCPRoute, TLSRoute).

Kod: Zdefiniowane w helm_release.traefik:
```
resource "helm_release" "traefik" {
  # ...
  values = [
    yamlencode({
      # ...
      providers = {
        kubernetesGateway = {
          enabled             = true # Włącza zarządzanie Gateway API
          experimentalChannel = true # Aktywuje eksperymentalne funkcje Gateway API
        }
        kubernetesIngress = {
          enabled = false # Wyłącza zarządzanie starszymi zasobami Ingress
```

#### Tworzenie Przestrzeni Nazw Traefika:

Opis: Dedykowana przestrzeń nazw (traefik) jest tworzona z etykietą shared-gateway-access: "true", która jest używana przez Gatewaya do kontrolowania, które Route'y mogą się do niego dołączać.

Kod: Definicja namespace'u:
```
resource "kubernetes_namespace" "traefik" {
  metadata {
    name = "traefik"
    labels = {
      shared-gateway-access = "true" # Etykieta do kontroli dostępu dla routów
```

## 3. Zasób Gateway (otel_gateway)
Definiuje główny punkt wejścia i porty, na których Traefik będzie nasłuchiwał.

Definicja Gatewaya:

Opis: Jest to główny zasób Gateway API, który Traefik będzie implementował.

Kod:
```
resource "kubernetes_manifest" "otel_gateway" {
  manifest = {
    apiVersion = "gateway.networking.k8s.io/v1beta1"
    kind       = "Gateway"
    metadata = {
      name      = "otel"
      namespace = kubernetes_namespace.otel_collector.metadata.0.name
    }
    spec = {
      gatewayClassName = "traefik" # Powiązanie z kontrolerem Traefik
      # ...
    }
```

#### Konfiguracja Listenerów (Słuchaczy):

Opis: Gateway definiuje porty i protokoły, na których nasłuchuje. Obejmuje to HTTP (8000), HTTPS (8443) z zakończeniem TLS oraz dynamiczne porty TCP/TLS.

Kod: Fragmenty z otel_gateway spec.listeners:
```
resource "kubernetes_manifest" "otel_gateway" {
  # ...
  manifest = {
    # ...
    spec = {
      # ...
      listeners = concat(
        [
          {
            name     = "web"
            port     = 8000
            protocol = "HTTP"
            allowedRoutes = { # Tylko route'y z przestrzeni nazw z etykietą `shared-gateway-access: true`
              namespaces = { from = "Selector", selector = { matchLabels = { shared-gateway-access = "true" } } }
            }
          },
          {
            name     = "websecure"
            port     = 8443
            protocol = "HTTPS"
            tls = {
              mode            = "Terminate" # Traefik deszyfruje ruch TLS
              certificateRefs = [for k in local.certs : { name = replace(k, ".", "-") }] # Referencje do certyfikatów
            }
            allowedRoutes = { # Podobnie jak dla HTTP
              namespaces = { from = "Selector", selector = { matchLabels = { shared-gateway-access = "true" } } }
            }
          }
        ],
        [
          for key, route in local.tcpRoutes : merge({ # Dynamiczne słuchacze TCP/TLS
            name     = key
            protocol = lookup(route, "protocol", "TCP")
            port     = route.port
            allowedRoutes = { kinds = [{ kind = "TCPRoute" }] } # Tylko TCPRoute mogą się dołączać
          },
          lookup(route, "tls", null) == null ? {} : { tls = route.tls }
          )
        ]
      )
    }
  }
}
```
## 4. Zarządzanie Certyfikatami TLS (Cert-Manager)
Cert-Manager automatyzuje pozyskiwanie i odnawianie certyfikatów TLS, które są następnie używane przez Traefik.

#### Definicja Zasobu Certificate:

Opis: Dla każdej domeny zdefiniowanej w local.certs, tworzony jest zasób Kubernetes Certificate, który instruuje Cert-Managera, aby wydał certyfikat.

Kod: Jest to zarządzane przez kubernetes_manifest.certs:
```
resource "kubernetes_manifest" "certs" {
  for_each = local.certs # Tworzy certyfikat dla każdej domeny
  manifest = {
    apiVersion = "cert-manager.io/v1"
    kind       = "Certificate"
    metadata = {
      name      = replace(each.key, ".", "-") # Nazwa certyfikatu (np. gw-observability-test-pndrs-de)
      namespace = kubernetes_namespace.otel_collector.metadata.0.name
    }
    spec = {
      dnsNames = [each.key] # Nazwa DNS dla certyfikatu
      duration = "8760h0m0s" # Ważność certyfikatu (1 rok)
      issuerRef = {
        kind = "ClusterIssuer"
        name = "edp-internal" # Odwołanie do wystawcy certyfikatów
      }
      secretName = replace(each.key, ".", "-") # Nazwa Secretu, gdzie certyfikat zostanie zapisany
    }
  }
}
```
## 5. Zasoby Route (syslogtcp_route)
Zasoby Route definiują faktyczne reguły kierowania ruchu z listenerów Gatewaya do usług backendowych.

#### Powiązanie Route z Gatewayem:

Opis: Każdy zasób Route (TCPRoute lub TLSRoute) jest powiązany z konkretnym listenerem na otel_gateway za pomocą parentRefs, kierując ruch do odpowiedniej usługi.

Kod: Fragment z kubernetes_manifest.syslogtcp_route:
```
resource "kubernetes_manifest" "syslogtcp_route" {
  for_each = local.tcpRoutes # Tworzy Route dla każdego zdefiniowanego routa TCP
  manifest = {
    apiVersion = "gateway.networking.k8s.io/v1alpha2"
    kind       = lookup(each.value, "ssl", {mode="Terminate"})["mode"] == "Passthrough" ? "TLSRoute" : "TCPRoute" # Rodzaj Route (TLS lub TCP)
    metadata = {
      name      = each.key
      namespace = kubernetes_namespace.otel_collector.metadata.0.name
    }
    spec = {
      parentRefs = [
        {
          name        = "otel" # Odwołanie do Gatewaya
          sectionName = each.key # Odwołanie do konkretnego listenera Gatewaya
        }
      ]
      rules = [
        {
          backendRefs = [
            {
              name = kubernetes_service_v1.collector.metadata.0.name # Docelowa usługa backendowa
              port = each.value.port # Port usługi backendowej
            }
```


## Podsumowanie Przepływu (z odniesieniem do kodu):

1. Żądanie DNS (helm_release.traefik service.annotations): Zewnętrzne zapytanie DNS trafia do ExternalDNS, który kieruje je do Service Traefika.

2. Ruch do Service Traefika (helm_release.traefik ports): Ruch dociera do klastra na odpowiednie porty wystawione przez Service Traefika.

3. Traefik jako Kontroler (helm_release.traefik providers.kubernetesGateway): Traefik (uruchomiony w namespace traefik) aktywnie nasłuchuje na zasoby Gateway API.

4. Gateway Listeners (kubernetes_manifest.otel_gateway listeners): Ruch jest kierowany do odpowiedniego listenera (web, websecure, lub dynamicznego TCP/TLS) zdefiniowanego w Gatewayu. Dla HTTPS, Cert-Manager (kubernetes_manifest.certs) dostarczył certyfikaty, które Traefik wykorzystuje do zakończenia TLS.

5. Reguły Routingu (kubernetes_manifest.syslogtcp_route rules): Zasoby Route instrukcje Traefikowi, jak kierować ruch z danego listenera.

6. Kierowanie do Backendu (kubernetes_manifest.syslogtcp_route backendRefs): Traefik ostatecznie przekierowuje ruch do docelowej usługi Kubernetes, np. otel_collector.