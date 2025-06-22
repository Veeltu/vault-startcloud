
### od Radka
tcpdump -s 0 -A dst port 4080

W Google Cloud, przekierowanie URL można ustawić tylko w regule ścieżki, która **nie** określa backendu ani akcji trasy.

## Zrozumienie zachowania

1. **Przekierowanie URL**:
    
    - Kiedy występuje przekierowanie URL, GCP wysyła odpowiedź `3xx` (taką jak `301 Moved Permanently`), która instruuje klienta (takiego jak `curl`) do wykonania nowego żądania do określonej lokalizacji (`Location`). W tym przypadku, nagłówki nie są stosowane do samej odpowiedzi przekierowania; będą one stosowane tylko do odpowiedzi z usługi backendowej.
    
2. **Akcje nagłówków**:
    
    - Blok `header_action` w twojej konfiguracji jest zaprojektowany do dodawania nagłówków do odpowiedzi pochodzących z usługi backendowej, a nie z samego przekierowania. Ponieważ odpowiedź przekierowania nie obejmuje bezpośrednio usługi backendowej, te nagłówki nie zostaną dodane.


```
sudo tcpdump -A -s 10240 'tcp port 4080 and (((ip[2:2] - ((ip[0]&0xf)<<2)) - ((tcp[12]&0xf0)>>2)) != 0)' | egrep --line-buffered "^........(GET |HTTP\/|POST |HEAD )|^[A-Za-z0-9-]+: " | sed -r 's/^........(GET |HTTP\/|POST |HEAD )/\n\1/g'
```

```
sudo tcpdump -A -s 10240 'tcp port 80 and (((ip[2:2] - ((ip[0]&0xf)<<2)) - ((tcp[12]&0xf0)>>2)) != 0)' | egrep --line-buffered "^........(GET |HTTP\/|POST |HEAD )|^[A-Za-z0-9-]+: " | sed -r 's/^........(GET |HTTP\/|POST |HEAD )/\n\1/g'
```

```
sudo tcpdump -A -s 10240 'tcp port 443 and (((ip[2:2] - ((ip[0]&0xf)<<2)) - ((tcp[12]&0xf0)>>2)) != 0)' | egrep --line-buffered "^........(GET |HTTP\/|POST |HEAD )|^[A-Za-z0-9-]+: " | sed -r 's/^........(GET |HTTP\/|POST |HEAD )/\n\1/g'
```

curl -I -H "Host: three.com" http://34.117.151.46