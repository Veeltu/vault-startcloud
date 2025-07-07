Tak, Twój zapis jest **poprawny** i zgodny ze specyfikacją Gateway API.

### Co oznacza `namespaces.from: Selector`?

- **`namespaces.from: Selector`** w sekcji `allowedRoutes` pozwala określić, z których namespace'ów mogą być dołączane trasy (`TCPRoute`, `HTTPRoute` itd.) do tego listenera Gateway.
- **Selector** pozwala ograniczyć możliwość podpinania tras tylko do tych namespace'ów, które mają określone label(e).

#### Przykład z Twojego fragmentu:
```yaml
allowedRoutes:
  kinds:
    - group: gateway.networking.k8s.io
      kind: TCPRoute
  namespaces:
    from: Selector
    selector:
      matchLabels:
        shared-gateway-access: "true"
```

**Znaczenie:**  
Tylko te namespace'y, które mają label `shared-gateway-access: "true"`, mogą tworzyć TCPRoute i podpiąć je do tego listenera Gateway[1][2][3][4].

#### Przykład namespace z odpowiednim labelem:
```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: my-app-ns
  labels:
    shared-gateway-access: "true"
```
Tylko w takim namespace TCPRoute (lub HTTPRoute, jeśli dodasz do kinds) zostanie zaakceptowany przez Gateway.

### Podsumowanie

- **`from: Selector`** + `selector.matchLabels` = tylko namespace'y z danym labelem mogą podłączać swoje Route do Gateway.
- To bardzo dobre rozwiązanie do kontroli dostępu w większych klastrach i multi-tenant.

Więcej przykładów i opisów znajdziesz w oficjalnej dokumentacji Gateway API[1][2][3][4].

[1] https://gateway-api.sigs.k8s.io/guides/multiple-ns/
[2] https://gateway-api.sigs.k8s.io/concepts/api-overview/?h=extension
[3] https://gateway-api.f5se.io/guides/multiple-ns/
[4] https://developer.konghq.com/kubernetes-ingress-controller/routing/cross-namespace/
[5] https://pplx-res.cloudinary.com/image/private/user_uploads/18333937/b2fe9808-aec0-4b4a-965f-69ec12feb55a/image.jpg
[6] https://gateway-api.f5se.io/guides/allowedroutes/
[7] https://konghq.com/blog/engineering/sending-traffic-across-namespaces-with-gateway-api
[8] https://isovalent.com/blog/post/tutorial-cross-namespace-routing-with-cilium-gateway-api/
[9] https://stackoverflow.com/questions/66479996/spring-cloud-gateway-route-to-service-in-another-namespace
[10] https://discuss.kubernetes.io/t/why-dont-we-consider-adding-namespace-attributes-to-the-selector-to-associate-cross-namespace-applications/26374
[11] https://kodekloud.com/community/t/need-explanation-about-allowedroutes-field-in-spec-of-gateway/477315
[12] https://gateway.envoyproxy.io/contributions/design/gatewayapi-translator/
[13] https://docs.solo.io/gloo-mesh-gateway/main/reference/api/core/
[14] https://docs.rs/k8s-gateway-api/latest/k8s_gateway_api/?search=GatewayController
[15] https://www.haproxy.com/documentation/kubernetes-ingress/gateway-api/tcproute/
[16] https://gateway-api.sigs.k8s.io/reference/spec/
[17] https://www.googlecloudcommunity.com/gc/Google-Kubernetes-Engine-GKE/Gateway-API-cross-namespace-routes-doesnt-work/m-p/737428
[18] https://www.googlecloudcommunity.com/gc/Google-Kubernetes-Engine-GKE/Gateway-API-cross-namespace-routes-doesnt-work/m-p/739472/highlight/true
[19] https://kubernetes.io/docs/concepts/services-networking/gateway/
[20] https://github.com/kubernetes-sigs/gateway-api/blob/main/conformance/base/manifests.yaml
[21] https://github.com/cilium/cilium/issues/34288