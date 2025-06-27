
#### command:

```
helm install otel-collector open-telemetry/opentelemetry-collector --namespace security --set mode=deployment --set image.repository="ghcr.io/open-telemetry/opentelemetry-collector-releases/opentelemetry-collector-k8s" --set command.name="otelcol-k8s" --set resources.limits.cpu=500m --set resources.limits.memory=512Mi --set resources.requests.cpu=250m --set resources.requests.memory=256Mi

```
***

Na podstawie przesłanego zrzutu ekranu, problem z uruchomieniem podu wynika z błędu przy pobieraniu obrazu kontenera:

```
Failed to pull image "ghcr.io/open-telemetry/opentelemetry-collector-releases/opentelemetry-collector-k8s:0.128.0": failed to resolve reference ... dial tcp: lookup artifactory.ctn.e-test.pndrs.de ... no such host
Error: ErrImagePull
Back-off pulling image ...
Error: ImagePullBackOff
```

## Analiza błędu

- **Błąd DNS:** Kubelet nie może rozwiązać adresu `artifactory.ctn.e-test.pndrs.de`, co sugeruje problem z DNS lub siecią w klastrze.
- **Obraz:** Próbujesz pobrać obraz `ghcr.io/open-telemetry/opentelemetry-collector-releases/opentelemetry-collector-k8s:0.128.0`. To oficjalny obraz z GitHub Container Registry, więc powinien być dostępny publicznie.
- **Przekierowanie przez proxy/artifactory:** Wygląda na to, że Twoja infrastruktura próbuje pobierać obrazy przez wewnętrzny serwer proxy lub artifactory (`artifactory.ctn.e-test.pndrs.de`). Jeśli ten host nie jest dostępny, pobieranie się nie powiedzie.

## Możliwe przyczyny

- **Brak dostępu do internetu** z węzłów K8s lub błędna konfiguracja proxy.
- **Problemy z DNS** w klastrze lub nieprawidłowa konfiguracja serwera proxy/artifactory.
- **Obraz nie istnieje** pod wskazanym tagiem (ale błąd sugeruje problem z siecią, nie z tagiem).

## Rozwiązania

- Sprawdź, czy węzły K8s mają dostęp do internetu i mogą rozwiązać adres `ghcr.io`.
- Jeśli korzystasz z proxy lub artifactory do pobierania obrazów, upewnij się, że jest on poprawnie skonfigurowany i dostępny.
- Jeśli nie chcesz korzystać z artifactory, sprawdź konfigurację węzłów (np. `/etc/docker/daemon.json` lub ustawienia proxy w K8s), aby obrazy były pobierane bezpośrednio z `ghcr.io`.
- Zweryfikuj, czy tag `0.128.0` istnieje w repozytorium obrazu.

## Podsumowanie

Błąd nie wynika z polecenia Helm, ale z problemów sieciowych lub proxy w Twoim środowisku K8s. Rozwiązanie wymaga poprawy dostępu do rejestru obrazów lub konfiguracji proxy.

[1] https://pplx-res.cloudinary.com/image/private/user_uploads/18333937/8cf7cb95-c654-4b62-b1b5-fc11ebb228ac/image.jpg
