Ten błąd jednoznacznie wskazuje, że **w namespace `network` nie istnieje sekret TLS o nazwie `gw-observability-test-pndrs-de`**. Traefik, jako kontroler Gateway API, oczekuje, że ten sekret będzie dostępny w tym właśnie namespace i będzie zawierał prawidłowy certyfikat oraz klucz prywatny[1][2][5].

---

## Jak stworzyć nowy sekret TLS w namespace `network`?

### 1. **Przygotuj certyfikat i klucz prywatny**
Musisz mieć pliki:
- **cert.pem** – certyfikat (PEM-encoded)
- **key.pem** – klucz prywatny (PEM-encoded)

Jeśli nie masz własnych, możesz wygenerować je na potrzeby testów:
```bash
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout key.pem -out cert.pem -subj "/CN=test.example.com"
```
*(Zastąp `test.example.com` swoją domeną lub dowolną nazwą.)*

---

### 2. **Utwórz sekret TLS w namespace `network`**
Użyj polecenia:
```bash
microk8s kubectl create secret tls gw-observability-test-pndrs-de \
  --cert=cert.pem \
  --key=key.pem \
  -n network
```
*(Jeśli pliki mają inne nazwy, dostosuj ścieżki.)*[2][3][6]

---

### 3. **Sprawdź, czy sekret został utworzony**
```bash
microk8s kubectl get secret gw-observability-test-pndrs-de -n network
```

---

### 4. **Traefik powinien automatycznie wykryć sekret**
Po utworzeniu sekretu Traefik Gateway API powinien przestać zgłaszać błąd i poprawnie utworzyć status Gateway.

---

> **Uwaga:**  
> Jeśli używasz cert-managera, utwórz zasób `Certificate` w namespace `network` z odpowiednią nazwą sekretu.  
> Jeśli sekret był w innym namespace, skopiuj go lub utwórz nowy w `network`[3][5].

---

**Podsumowanie:**  
Wystarczy utworzyć sekret TLS o nazwie `gw-observability-test-pndrs-de` w namespace `network`, a Traefik przestanie zgłaszać ten błąd.

[1] https://doc.traefik.io/traefik/https/tls/
[2] https://kubernetes.io/docs/reference/kubectl/generated/kubectl_create/kubectl_create_secret_tls/
[3] https://www.strongdm.com/blog/kubernetes-secrets
[4] https://snyk.io/blog/setting-up-ssl-tls-for-kubernetes-ingress/
[5] https://doc.traefik.io/traefik-hub/api-gateway/secure/tls/cert
[6] https://kubernetes.io/docs/reference/kubectl/generated/kubectl_create/kubectl_create_secret/
[7] https://doc.traefik.io/traefik/routing/providers/kubernetes-gateway/
[8] https://traefik.io/blog/getting-started-with-kubernetes-gateway-api-and-traefik/
[9] https://doc.traefik.io/traefik/getting-started/quick-start-with-kubernetes/
[10] https://github.com/kubernetes/ingress-nginx/issues/2170