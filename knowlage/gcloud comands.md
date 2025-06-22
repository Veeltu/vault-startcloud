

login
```
gcloud auth login
```
**Sprawdzenie instancji VM**:bash
```
gcloud compute instances list
```
**Sprawdzenie wszystkich zasobów w projekcie**:  
```
gcloud resource-manager resources list
```
Test backend
```
gcloud compute backend-services describe bluebackendservice --global --project=test-mtls-442109
```
