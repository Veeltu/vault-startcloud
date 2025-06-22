#docker

Oto kroki, aby pociągnąć obraz Fedora, uruchomić kontener i pracować na nim:

### 1. Zainstaluj Docker

Jeśli Docker nie jest zainstalowany na Twoim systemie, musisz go zainstalować. Na systemie Fedora możesz to zrobić za pomocą następującego polecenia:

```bash
sudo dnf install docker-ce docker-ce-cli containerd.io docker-compose-plugin docker-compose -y
```

### 2. Uruchom i włącz usługę Docker

Po instalacji uruchom i włącz usługę Docker:

```bash
sudo systemctl start docker
sudo systemctl enable docker
```

### 3. Pobierz obraz Fedora

Pobierz obraz Fedora za pomocą polecenia:

```bash
docker pull fedora:latest
```

### 4. Uruchom kontener

Uruchom kontener z obrazu Fedora:

```bash
docker run -it fedora bash
```

To polecenie utworzy nowy kontener z obrazu Fedora i uruchomi w nim powłokę Bash. Możesz pracować wewnątrz kontenera.

### 5. Pracuj w kontenerze

Wewnątrz kontenera możesz instalować pakiety, edytować pliki itd. Jeśli chcesz zachować zmiany, musisz utworzyć nowy obraz za pomocą polecenia `docker commit`.

### 6. Zapisz zmiany (opcjonalnie)

Jeśli chcesz zachować zmiany wprowadzone w kontenerze, możesz utworzyć nowy obraz:

1. Wyjdź z kontenera, naciskając `Ctrl + D`.
2. Znajdź ID kontenera:
   ```bash
   docker ps -a
   ```
3. Utwórz nowy obraz:
   ```bash
   docker commit  moj_fedora
   ```

Teraz możesz uruchomić nowy kontener z Twoim obrazem:

```bash
docker run -it moj_fedora bash
```

### 7. Zatrzymaj i usuń kontener

Po zakończeniu pracy zatrzymaj i usuń kontener:

```bash
docker stop 
docker rm 
```

Pamiętaj, że kontenery są tymczasowe, a zmiany nie są zapisywane automatycznie. Jeśli chcesz zachować zmiany, musisz utworzyć nowy obraz.

Citations:
[1] https://manpages.ubuntu.com/manpages/bionic/man1/docker-image-pull.1.html
[2] https://www.dogtagpki.org/wiki/Fedora_Container
[3] https://docs.docker.com/reference/cli/docker/image/pull/
[4] https://developer.fedoraproject.org/tools/docker/docker-usage.html
[5] https://fedoramagazine.org/container-technologies-fedora-docker/
[6] https://www.atlantic.net/vps-hosting/how-to-install-and-use-docker-on-fedora/
[7] https://developer.fedoraproject.org/tools/docker/docker-images.html
[8] https://hub.docker.com/_/fedora
[9] https://docs.fedoraproject.org/en-US/iot/run-container/
[10] https://hub.docker.com/r/amd64/fedora/
[11] https://labex.io/tutorials/docker-how-to-pull-a-docker-image-411581
[12] https://www.redhat.com/en/blog/you-initiate-docker-pull
[13] https://docs.docker.com/desktop/setup/install/linux/fedora/
[14] https://github.com/fedorapackaging/docker-images
[15] https://fedoramagazine.org/how-to-build-fedora-container-images/
[16] https://docs.docker.com/engine/install/fedora/

---
Odpowiedź od Perplexity: pplx.ai/share