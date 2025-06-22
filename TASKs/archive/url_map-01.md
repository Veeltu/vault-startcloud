Proszę przygotować terrafom dla konfiguracji URL mapy dla HTTPS LB w GCP. URL mapa powinna być dynamiczna w zależności od poszczególnych path oraz ilości http header, uwzględniając jako parametr przełączanie w scenariuszu blue/green.

Linki:  
[![](https://www.gstatic.com/devrel-devsite/prod/vc38a15f5a89ceb3dee5ad4d2d7b57b718d8556c0e3724a12c131a2b9d037d8c7/cloud/images/favicons/onecloud/favicon.ico)Mutual TLS overview  |  Load Balancing  |  Google Cloud](https://cloud.google.com/load-balancing/docs/mtls)  
[![](https://registry.terraform.io/images/favicons/favicon-16x16.png)Terraform Registry](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/compute_url_map#example-usage---url-map-traffic-director-route)  
[![](https://registry.terraform.io/images/favicons/favicon-16x16.png)Terraform Registry](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/compute_url_map#example-usage---url-map-traffic-director-path-partial)

1. dynamiczna mapa od poszczegolnych path 
2.  parametr blue/green
3. iterowanie przez headery + replace
***
