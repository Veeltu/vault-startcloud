wOczywiście! Oto gotowe polecenia do utworzenia wszystkich potrzebnych Secretów **do testów** dla Twoich envów, z użyciem `sudo microk8s kubectl` i namespace `network`:

---

## 1. **Secret dla Kafki**

```sh
sudo microk8s kubectl create secret generic kafka-credentials -n network \
  --from-literal=username=testuser \
  --from-literal=password=testpass
```

---

## 2. **Secret dla SNMP**

```sh
sudo microk8s kubectl create secret generic snmp-credentials -n network \
  --from-literal=auth-password=snmpauth \
  --from-literal=privacy-password=snmppriv
```

---

## 3. **Sprawdzenie, czy secrety powstały:**

```sh
sudo microk8s kubectl get secrets -n network
sudo microk8s kubectl describe secret kafka-credentials -n network
sudo microk8s kubectl describe secret snmp-credentials -n network
```

---

## 4. **Podsumowanie**

- **Nie musisz restartować klastra ani VM.**
- Wartości możesz oczywiście zmienić na dowolne testowe.
- Po wdrożeniu deploymentu z envami, sprawdź w podzie:
  ```sh
  sudo microk8s kubectl exec -it  -n network -- env | grep KAFKA
  sudo microk8s kubectl exec -it  -n network -- env | grep SNMP
  ```

---

**To wszystko!**  
Teraz Twoje envy w podzie będą miały wartości z tych Secretów.