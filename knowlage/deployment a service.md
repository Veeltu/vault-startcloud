
W Kubernetes istnieją dwa różne obiekty: **Deployment** i **Service**. Oba są kluczowe, ale pełnią zupełnie inne role:

---

## 1. `kubernetes_deployment`

- **Opis:** Deployment zarządza cyklem życia aplikacji uruchamianej w postaci Podów (kontenerów) – decyduje ile replik aplikacji ma działać, kiedy je zaktualizować, restartować, itp.
- **Cel:** Zapewnia, że określona liczba instancji (Podów) Twojej aplikacji jest zawsze uruchomiona.
- **Przykład:** Jeśli chcesz, by zawsze działały 3 kopie Twojego backendu, tworzysz Deployment z `replicas: 3`.

---

## 2. `kubernetes_service`

- **Opis:** Service to obiekt, który **udostępnia aplikację** (Pody) w sieci Kubernetes – zarówno innym aplikacjom w klastrze, jak i (opcjonalnie) światu zewnętrznemu.
- **Cel:** Zapewnia stabilny adres IP i DNS dla Twojej aplikacji, niezależnie od tego, ile razy Pody będą się restartować czy zmieniać IP.
- **Przykład:** Jeśli masz Deployment z 3 Podami, Service pozwala innym aplikacjom (lub użytkownikom) łączyć się do nich przez jeden stały adres.

---

## Jak to działa razem?

- **Deployment** tworzy i zarządza Podami (np. 3 kopie aplikacji).
- **Service** wskazuje na te Pody i umożliwia dostęp do nich przez jeden adres (load balancing wewnątrz klastra).

---

### Przykład praktyczny

1. Tworzysz `Deployment` dla swojej aplikacji.
2. Tworzysz `Service`, który wskazuje na Pody tego Deploymentu.
3. Inne aplikacje lub użytkownicy łączą się przez Service, a Kubernetes rozdziela ruch do Podów.

---

**Podsumowanie:**  
- **Deployment** = zarządzanie cyklem życia Podów (ile, kiedy, jak).
- **Service** = udostępnianie aplikacji w sieci (load balancing, stały adres)[2].

Oba te obiekty są standardem w każdej aplikacji uruchamianej na Kubernetes.

[1] https://osm.etsi.org/docs/user-guide/latest/15-k8s-installation.html
[2] https://kubernetes.io/docs/concepts/services-networking/service/index.html
[3] https://opentelemetry.io/docs/collector/configuration/
[4] https://opentelemetry.io/docs/demo/kubernetes-deployment/
[5] https://blog.ovhcloud.com/wp-content/uploads/2020/11/2020-10-29-OVHcloud-Tech-Talks-S02E02-Building-a-cloud-network-function-platform-using-kubernetes.pdf
[6] https://www.youtube.com/watch?v=dc4LtErreVg
[7] https://github.com/ovn-kubernetes/ovn-kubernetes
[8] https://kubernetes.io/docs/concepts/cluster-administration/addons/
[9] https://github.com/OpenCloudOps/k8s-demo
[10] https://osm.etsi.org/gitlab/osm-doc/osm-user-guide/-/blame/cfeaaad26aa3e4fc9e7941da4e141bcfd9f36dcf/15-k8s-installation.md
[11] https://docs.redhat.com/en/documentation/red_hat_openstack_platform/17.1/html/installing_and_managing_red_hat_openstack_platform_with_director/assembly_provisioning-and-deploying-your-overcloud
[12] https://opentelemetry.io/docs/kubernetes/helm/demo/
[13] https://docs.redhat.com/en/documentation/openshift_dedicated/4/html/networking/ovn-kubernetes-network-plugin
[14] https://www.youtube.com/watch?v=Wyr9sE76fH8
[15] https://github.com/kubeovn/kube-ovn


W Kubernetes **porty są ustawiane zarówno w Deployment, jak i w Service**, ale każdy z tych obiektów robi to z innego powodu i w innym celu.

---

## 1. Porty w Deployment

- **Cel:**  
  - Określają, na jakich portach **Pody (kontenery) będą nasłuchiwać** na żądania wewnątrz siebie.
  - Definiują, jakie porty są otwarte w kontenerze i jak aplikacja w kontenerze jest skonfigurowana do komunikacji.
- **Jak to działa:**  
  - W sekcji `containers` w Deployment określasz porty, które mają być wystawione przez kontener.
  - Przykład:
    ```yaml
    containers:
    - name: my-app
      image: my-app:latest
      ports:
      - containerPort: 8080
    ```
  - Oznacza to, że kontener nasłuchuje na porcie 8080.

---

## 2. Porty w Service

- **Cel:**  
  - Określają, **na jakim porcie Service będzie nasłuchiwać** na żądania z zewnątrz lub z innych aplikacji w klastrze.
  - Definiują, jak ruch będzie przekierowywany z Service do Podów.
- **Jak to działa:**  
  - W sekcji `ports` w Service określasz:
    - `port` – port, na którym Service będzie nasłuchiwać (np. 80).
    - `targetPort` – port, na który ruch będzie kierowany do Podów (musi odpowiadać portowi z Deployment).
  - Przykład:
    ```yaml
    ports:
    - port: 80
      targetPort: 8080
    ```
  - Oznacza to, że ruch przychodzący na port 80 Service zostanie przekierowany na port 8080 Podów.

---

## Podsumowanie

- **Deployment** – ustawiasz port, na którym nasłuchuje aplikacja w kontenerze.
- **Service** – ustawiasz port, na którym nasłuchiwać będzie Service oraz port, na który ruch będzie przekierowywany do Podów.

Dzięki temu ruch z zewnątrz lub z innych aplikacji w klastrze trafia do właściwego miejsca w Twojej aplikacji.  
**Service** jest jak „brama” – przekierowuje ruch na właściwy port w kontenerze, który został ustawiony w Deployment.